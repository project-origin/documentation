


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

TODO: Afhængighed fra ledger til "Public services" er forkert - pilene bør kun pege fra services til ledger og ikke den anden vej rundt.

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
