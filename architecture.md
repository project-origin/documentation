


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

At the core of the platform is the [shared verifiable storage (blockchain)](blockchain-protocols.md). The blockchain is intended to be a common place where end-users can verify their GGOs. In this prototype, this is currently implemented using Hyperledger Sawtooth, but a replacement is already under way.

In the Danish Setup there is a [DataHub Service](#datahub-service). The responsibility for this service is to bridge the danish DataHub and its measurements with the prototype, and add data to the verifiable storage. This could also be replaced with a decentralized solution to let meters write directly to the verifiable storage if a issuing body so wishes.

Next public service is the [Account Service](#account-service), this was original an way to abstract away the complexity of dealing directly with the ledger, wallet and keys to a more friendly account API. But there are considerations about making it the central access entry-point to the platform. 

OAuth2 is used for authorization, this enables the users to be able to delegate access to their data to third party clients.

An example application has also been created, this application uses many of the APIs created in the Account and DataHub service, and enables a user to do basic functionality and visualization on top of their data available in the platform. But the goal is to have third-parties create rich end user applications on top of the platform.


![Archictechtural overview](figures/arch-overview.png)

(Temp image)

TODO: Afhængighed fra ledger til "Public services" er forkert - pilene bør kun pege fra services til ledger og ikke den anden vej rundt.

# <a id="datahub-service">DataHubService</a>

This service is responsible for bridging the danish DataHub with the rest of the platform. DataHub exposes metering points and measurements from the danish consumers via an API, and DataHubService uses this to fetch data for the platform's users. It imports measurements from DataHub and publishes them to the blockchain while at the same time issues GGOs to the blockchain for production meters.

DataHubService also exposes a few API endpoints to fetch metering points, measurements, and GGOs, and to create publicly available disclosed datasets.

To enable importing of data, users must onboard to DataHub via DataHubService. This process submits an authorization which allows DataHubService to import data for a consumer thus make it available to the rest of the platform.

DataHubService is [open-source and can be found on GitHub](https://github.com/project-origin/datahub-service).

# <a id="account-service">AccountService</a>

This service was intended as a way to abstract away the complexity of dealing directly with the blockchain, wallet, and keys to a more friendly account-oriented API. It keeps balance of GGOs and their state, while also exposing a number of endpoints to transfer GGOs to other accounts, and retire GGOs to measurements. Effectively AccountService has a copy of the GGOs on the blockchain to be able to index them for querying and other practical use-cases.

# <a id="account-service">Example Application</a>

TODO

# <a id="account-service">Authorization</a>

TODO

<!-- 

[Measurements](measurements.md)


[GGO](ggo.md)




- blockchain
- distributed setup
- wallets
- measurements
- ggo -->
