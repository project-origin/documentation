
# Core decisions 

There was a number of early decisions made, which all has formed how this platform has turned out.

Each of these decisions will be further explained in the following chapters.

- The platform is a learning vehicle to fail-fast.
- The platform must be open-source.
- The platform should liberalize and enable even access to data.
- The platform should enable innovation.
- The platform must be opt-in.
- The platform will settle each individual electricity meter with GGOs.
- The platform will be based on the measurements peoples bill are settled upon.
- The platform will use the highest granular data available.


## Learning vehicle

This prototype is developed for both the participating parties to be able to see how a future platform would look and work, while we at the same time run into many of the core issues that arise. 

While in this early prototype stage, it is fairly easy to change the way core elements of the platform works or performs with minimal impact for participating parties, which enables us to receive feedback and quickly react to the feedback and try it out for all participating parties.

Issues are also far less expensive (time and monetary wise) to explorer and try different solutions to while in an early prototype experiment, than while developing a production grade solution.

## Open source

It was essential to create the platform in an open environment so everyone can participate and perform oversight on how things are done. We hope the learnings from the platform can help with more innovation in the sector as the project progresses.

## Liberalize data

A core aspect of the platform is to enable users as much insight into their data as possibly. All the users data must be made available on open APIs so they can access their data in the way the wishes. Users must also be able to delegate access to any of their data and APIs so any third party can act on behalf of the user.

## Enable innovation

By providing low friction was for third parties to integrate to the solution, we want to enable new solutions to be build on top of the platform and through it enable innovation.

## Opt-in

Because of the nature of an experimental platform as this, we want people to actively chose to be a part of it as long as it is a prototype project. Users rights to their own data is extremely important, and we want to safeguard their rights any way possibly.

We do opt-in by using the third party API on an existing service called [Eloverblik](https://eloverblik.dk/) where users already have access to their measurements. Here users can sign in with their national digital signature and grant this prototype access to their data.

## Individual settled meters

GGOs will be used to settle the exact amount of electricity used on each meter, this will enable the most granular documentation of the origin of the electricity as possibly.

## Based on measurements

In Denmark we settle and bill the metering points based on two virtual metering-points consumption (E17) and production (E18).

The reason why we call these virtual is that calculations done on the physical meters are done, these calculations depends on the individual installation. But without going into deep explanation, the E17 and E18 are the actual amount used and produced, no matter how the individual installation is configured at the specific location.

We use these measurements as the basis for everything in this platform. These measurements are available on an hourly resolution for all of Denmark, and are available in Wh. And users can already access their data directly on [Eloverblik](https://eloverblik.dk/).

All GGOs is issued based on production measurements together with a metadata set which denotes what type of production that is connected.

All consumption measurements are then settled using the GGOs to document the origin og the electricity. And if no GGOs are used, then the residual mix in the grid will be used.

## Highest granular data

Currently in Denmark the most granular available settlement data is the hourly measurements in Wh.

The is a goal to go to a higher granularity, and physical measurements for 15 min is available for all of DK1. But DK2 is not yet available, and all the calculations from physical to settlement data would have to be recreated, which is not in the scope of the platform.














<!-- 


The graft below shows the renewable production for 3. and 4. of april 2019. The blue line is the actual production in a 5 minute resolution and the orange is the mean hourly production.

![](figures/april-production-hour.png) 

As can be seen, there still is a deviation between the produced and consumed renewable electricity,  -->