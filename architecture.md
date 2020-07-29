


<!-- Background
Base design goals

architectural design
- blockchain
- distributed setup
- wallets
- measurements
- ggo

- account service
- architectural view -->



# Architectural Overview

Here we will outline an overview of the architecture.

At the core of the platform is the [shared verifiable storage](#verifiable-storage). The shared verifiable storage is intended to be a common place where end-users can verify their GGOs. In the prototype, this is currently done by Hyperledger Sawtooth, but a replacement is already under way.

In the Danish Setup there is a [DataHub Service](#datahub-service). The responsibility for this service is to bridge the DataHub and its measurements with the platform, and add data to the verifiable storage. This could also be replaced with a decentralized solution to let meters write directly to the verifiable storage if a issuing body so wishes.

Next public service is the [Account Service](#account-service), this was original an way to abstract away the complexity of dealing directly with the ledger, wallet and keys to a more friendly account API. But there are considerations about making it the central access entry-point to the platform. 

OAuth2 is used for authorization, this enables the users to be able to delegate access to their data to third party clients.

An example application has also been created, this application uses many of the APIs created in the Account and DataHub service, and enables a user to do basic functionality and visualization on top of their data available in the platform. But the goal is to have third-parties create rich end user applications on top of the platform.


![Archictechtural overview](figures/arch-overview.png)

(Temp image)

# <a id="verifiable-storage">Shared verifiable storage</a>

This is the core part of the platform, where all measurements and GGOs are persisted and are verifiable.

This is currently based on a blockchain, [as explained here](blockchain.md). The current blockchain is [Hyperledger Sawtooth](https://www.hyperledger.org/use/sawtooth) set up in a private permissioned configuration. 

The intent is for the blockchain to be Public Permissioned, but because of privacy concerns the public part is currently disabled in the platform. It is hard to insure privacy when there is a low number of actors on the blockchain, similar to hiding a needle in a haystack of two straws.

Since the initial design, and after some of the considerations highlighted in the [trust section](trust.md), we are currently looking at alternative to Hyperledger Sawtooth. This could be something like IOTA, or moving the data off ledger entirely while insuring its verifiability.

## Ownership

All blocks on the ledger are signed with a private key for owners to be able to prove their ownership of the block, which can contain either a measurement or a GGO. The signed block can be validated as the owner writing the block (transaction) using their public key.

[BIP32](https://github.com/bitcoin/bips/blob/master/bip-0032.mediawiki) (developed by the BitCoin community) enables us to deterministic create unique public/private keys for every single block. This prevents the public from correlate ownership of blocks on the ledger, thus increasing anonymity without hiding data in the block. BIP32 further enables us to give an extended public key to the party creating the blocks, so they can continuously create blocks where only the owner has write access over.

The extended key is also used to generate the corresponding addresses. More on this in the [DataHub service]('TODO').

## Namespace and addressing

In Hyperledger Sawtooth it is possible to isolate different parts of a system in namespaces. The first three bytes of each address denotes the namespace. Currently the platform uses three namespaces: one for measurements, one for GGOs, and one for settlements.

Each measurement has its address deterministically calculated based on a combination of the owners extended public child key and the measuring begin-timestamp. This coupled with the namespace enables us to know in advance where to place a GGO originating from a measurement; we calculate the same address, but in a different namespace. This way everyone can easily look up the production measurement which were the basis for an issued GGO. The same is done when calculating a settlement's addresse, only in this case the basis is a consumption measurement's address, but in the settlement namespace.

The GGO and Settlement namespaces both have references to the Measurement namespace; issued GGOs refer to the production measurement its based upon, and settlements refer to the consumption measurement they are settling on. The Measurement namespace does not have references to the other namespaces; this was done to isolate the functionality in the GGO part as much as possible, since the measurements could potentially e used for other things in the future.

## Measurements

Measurements from electricity meters are written to the verifiable storage. They are later used for either issuing GGOs (production) or to settle GGOs to a meter (consumption).

Each measurement stores a series of key information. Below is an example of a formatted JSON body of a block:

    {
        "begin": "2020-07-22T10:00:00Z",
        "end": "2020-07-22T11:00:00Z",
        "sector": "DK1",
        "type": "PRODUCTION",
        "amount": 1419
    }


### Time-span - begin and end

Each measurement has a begin and end timestamp in ISO-8601 format. 

The reasoning behind having both a start and end (start and duration was also an option) is that we can dynamically change the time-span of each measurement and GGO without having to change the underlying rules in the platform.

More information can also be read in [GGO time-span](#ggo-time)

### Sector

For geographical correlation we have added a sector. 

Currently sectors are defined as the price-area, in Denmark DK1 and DK2, but could also be at a more granular level if there are congestion within a single price-area. 

This is why the word **"sector"** was chosen as not to predefine what it is.

### Type

This can either be CONSUMPTION or PRODUCTION.

This simply denotes if the amount of energy was produced and sent to the grid or consumed from the grid.

### Amount

This is the amount of Wh consumed or produced in the time-span for the meter. This is always a whole number (integer).

### GSRN (Meter-id)

The observant reader probably recognizes that there is no information on what GSRN the measurement comes from. If this information had been stored on the ledger (hashed or not), there would be a possibility to do a correlation attack or to brute-force reverse the GSRN, hence it is not stored on the ledger itself.

The owner can prove the originating GSRN of a measurement by granting access to DataHubService, which exposes the GSRN number for a measurement via the API.

[An earlier concept](old.md#public-measurement) showed an example on how to do it in a encrypted stream of similar to an <a href='https://blog.iota.org/introducing-masked-authenticated-messaging-e55c1822d50e'>Restricted IOTA MAM streams</a>, but there is an issue with GDPR and not being able to delete one's own data.


## GGO - Granular Guarantee of Origin

Every GGO in the platform denotes the origin of an amount of electricity produced. Each GGO is issued with a reference to the production measurement its based upon.

Below is an example of a formatted JSON body of a block:


    {
        "origin": "%address of where this GGO came from%"
        "begin": "2020-07-22T10:00:00Z",
        "end": "2020-07-22T11:00:00Z",
        "amount": 1419
        "sector": "DK1",
        "tech_type": "T020002",
        "fuel_type": "F01050100",
        "next": null
    }

### Origin

This field always contains an address on the ledger where this GGO originated. There are two difference types of GGO origin.

The first possibility is that the GGO was issued based on a production measurement. In this case the "origin" will contain the address of the measurement, and amount, begin, end, and sector will always be equal to those of the measurement.

The second possibility is that the GGO is a result of another GGO being transferred or split into smaller GGOs. In this case the "origin" will contain the address of the parent GGO. If it is a transferred GGO, then amount, begin, end and sector must still be equal to its parent. If it is a split, then amount would be a smaller amount than its parent, but begin, end, and sector must be equal to its parent.

It is possible to distinguish between these two possibilities by looking at the namespace of the origin.

### <a id="ggo-time">Begin and End</a>
In Project Origin we chose each GGOs to have a predetermined length of time (Market resolution) instead of a predetermined size (GO 1 MWh).

The reasoning behind locking the length of each GGO is that electricity is volatile and only exists while it is produced, temporal correlation between production and consumption is therefore of high importance.

The grafh below shows the renewable production for the 3. and 4. of april 2019. The blue line is the actual production in a 5 minute resolution and the orange is the mean hourly production.

![](figures/april-production-hour.png) 

The grafh visualizes the volatility of renewable production, within 12 hours the production fell from 2.500 MWh to 750 MWh, or in other words fell to 30% of its peak. 

Hourly resolution was chosen since it is what we currently have available for all of measurement points in Denmark through [DataHub](datahub.md) and it being the current market resolution. Work is in progress to move it to a lower resolution in the future (15 or 5 minute)

To enable higher resolution in the future, each GGO has both a **begin** and **end** timestamp. This way, when higher resolutions (15 min) becomes available, four GGOs are issued instead of a single for each hour, with the corresponding begin and end time.

The dates are in ISO-8601 format, the begin is inclusive and the end exclusive. This will be interpreted as a single GGO for 1 hour where the next one will begin at the end of the previous one.

### Amount

We want to be as granular as our data will allow, and since we measure all meters in watt-hour, this is also what we will issue all GGOs on. 

The amount on each GGO is therefore in watt-hour and is an whole integer number.

Example: a GGO containing 1419 watt-hour

    "amount": 1419

### Sector

For geographical correlation we have added a sector to each GGO. 

Currently these sectors are defined as the price-area, in Denmark DK1 and DK2.

But this could also be at a more granular level if there are congestion within a single price-area. 

This is why the word **sector** was chosen as not to predefine the limit.

Example:

    "sector": "DK1"

### tech_type and fuel_type

Based on the work from <a href="https://www.aib-net.org/sites/default/files/assets/eecs/facts-sheets/AIB-2019-EECSFS-05%20EECS%20Rules%20Fact%20Sheet%2005%20-%20Types%20of%20Energy%20Inputs%20and%20Technologies%20-%20Release%207.7%20v5.pdf">AIB (Association of Issuing Bodies) fact 5</a> on technology and fuel codes, we use their codes as used in the GO system on the GGOs.

Their tech and fuel codes are an already established standard and is a verbose way to describe the origin of the electricity.

Each single GGO has a tech_type and fuel_type field appended to them:

Example: Offshore wind power.

    "tech_type": "T020002"
    "fuel_type": "F01050100"

### Next

The field is used to describe what has happened with the GGO. 

When this field is empty, then the GGO has not been used. 

When any action on the GGO is done, the field is set with an object. 

The action field has 3 possibilities: TRANSFER, SPLIT or RETIRE each denoting what sort of action was performed.

The addresses field points to the addresses where the GGO was used. On a transfer the list will always contain a single address pointing to the new address. On a split it will point to multiple address where the GGO was split to. On retire, it will point to the Settlement where it was used.

    {   
        "action": "SPLIT"
        "addresses": [
            "%address 1%",
            "%address 2%"
        ]
    }

## Settlement

The settlement object is the way we document what GGOs has been retired to a specific measurement.

    {
        "measurement": "%address to a consumption measurement%"
        "parts" : [
            {
                "ggo": "%the address of the retired ggo%"
                "amount": 123
            },
            {
                "ggo": "%the address of the retired ggo%"
                "amount": 642
            }
        ]
    }

### Measurement

This field is a reference to the consumption measurement, it is not needed strictly needed, since the measurement has the same address but in a different namespace.

### Parts

Each part in the list is a GGO retired to the settlement. The amount is included so that it is not necessary to read all address to calculate wether the total is larger than the measurement, since it is not allowed to retire more GGOs to a measurement, than the amount of electricity used.

But every time a GGO is retired to a measurement it is added here but the transaction processors.

## Transaction Processors

For each action that can be performed on the ledger, a transaction processor is defined. The transactions we have defined for this platform is:

- Publish measurement
- Issue GGO 
- Transfer GGO
- Split GGO
- Retire GGO
- Update settlement

Each of these transactions is processed, and the validity is validated by the transaction processors which is hosted as part of the blockchain on each node.

### Publish measurements - (Only issuing body)

This transaction publishes a measurement to the ledger, currently this is only allowed by the issuing body, that guarantees the validity of the measurement. 

### Issue GGO - (Only issuing body)

This is often called in the same batch as publish measurement by the issuing body, in case it is a production measurement.

This transaction creates a GGO on the address, with the values from the measurement and some metadata about the technology and fuel used to produce the electricity.

### Transfer GGO 

With the private key, it is possibly to transfer the ownership of a GGO. This creates a new GGO at the receivers address, and sets the GGO next to point to that address.

This action can also be performed with a split of only 1, which translates to it being redundant, and will be removed.

### Split GGO 

The owner can split a GGO into 1 to n parts. The smallest part can be a single wh, as the amount only can be a whole number. The transaction request contains a list of the receiving addresses on the ledger, and is signed with the owners private key.

This creates the new GGOs as well as sets the next value of the GGO being split.

### Retire GGO

This retires a GGO to a specific settlement. This should always be done in the same batch as the update settlement transaction, so if the update settlement fails the retire is rolled back.

The transactions updates the next value to point to the settlement.

### Update settlement

This transactions creates or updates a settlement for a corresponding consumption measurements. It validates that a GGO has been retired to the settlement, and adds it to the settlement together with the corresponding addresses.


# <a id="datahub-service">DataHub service</a>

!todo!

# <a id="account-service">Accounts service</a>

!todo!

<!-- 

[Measurements](measurements.md)


[GGO](ggo.md)




- blockchain
- distributed setup
- wallets
- measurements
- ggo -->
