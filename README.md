# Project Origin

Project Origin is a prototype project where the goal is to document the origin of electricity on the same resolution as the electricity market, currently hourly.

If you want some of the [Background info for the project it can be found here](background.md)

Early in the project a series of [Core decisions](core-decisions.md) was made.

You can find more about the [architecture here](architecture.md).

# Eloprindelse

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





