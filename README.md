# Project Origin

Project Origin is an Energinet prototype-project with the goal of providing a way to prove the origin of consumed electricity. It uses the same resolution as the electricity market, currently hourly, instead of the yearly resolution used by the current Guarantee of Origin system (GO), hence we call it Granular Guarantee of Origin (GGO).

The active prototype is currently scheduled to run until November 2020, where the goal is to learn and further develop the prototype, and to harvest all the learned experiences into a possibly future production-ready solution. We welcome as many participants and as much feedback as possible, and will do our best to incorporate feedback into improving the prototype as fast as possible.

Key features of the prototype include:

- Issuing of GGOs based on actual energy production
- Setting up transfer agreements to [continuously] transfer issued GGOs to trading partners
- Retiring of GGOs to prove the origin of consumed energy
- Generating environment declaration based on retired GGOs, including detailed emission data and source technology
- Onboarding users via Eloverblik.dk to access their actual measurement data
- Using a blockchain as ledger for public verifying data

## Participating in the prototype

Sign up is open for everyone at [Eloprindelse.dk](https://eloprindelse.dk).

## Project documentation

- Project background and considerations
  - [Background info on the project](background.md)
  - [Core decisions made early](core-decisions.md)
  - [Should we use blockchain?](blockchain.md)
  - Further possibilities
  - Limitations
- Prototype implementation
  - [Architectural overview](architecture.md)
  - Shared verifiable storage (blockchain) protocols

# Eloprindelse

```diff
- Forstår ikke lige helt det første paragraf i det her afsnit. Har flytet de andre paragraffer op i første afsnit ovenfor.
```

This project is the basis for the active Danish prototype of the platform called [Eloprindelse](http://eloprindelse.dk). 

The active prototype is currently scheduled to run until November 2020, where the goal is to learn and develop the prototype, and to harvest all the learned experiences into a possibly future production solution. 

We welcome as many participants and feedback as possibly. We will do our best to incorporate feedback improves the prototype.

# Further possibilities

Based on this structure, it should be fairly easy to later add more functionality to the platform. This could be how to transfer GGO between sectors, append more meta data or other possibilities.

A [environmental module]() is currently in the works, where we use data from the environmental and electricity declarations in Denmark to make an hourly electricity declaration based on the data from the GGOs with an hourly  residual mix based on the same dataset as the current declarations.

A [forecast module]() is also on the drawing-boards, this will enable producers, resellers and consumers to share information about production, consumption and available GGOs ahead of the actual issuing of the GGOs. The hope is to enable a balancing of renewable energy.

# Cooperation
 
This project is open source, and our wish is for everyone interested to be part of the project and platform.

You are also welcome to fork it and use it for your own purposes, but we would hope you want to be a part of a common platform.

# Limitations

Hyperledger Sawtooth are known to have some limits on the number of transactions it can process. 

Therefore this can not be taken as a production ready solution, but more as a proof of concept on how to do Granular Guarantee of Origin.





