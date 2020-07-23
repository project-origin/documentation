
# GGO - structure

## GGO time-span
In Project Origin we chose each GGOs to have a predetermined length of time (Market resolution) instead of a predetermined size (GO 1 MWh)

The reasoning behind locking the length of each GGO is that electricity is volatile and only exists while it is produced, temporal correlation between production and consumption is therefore of high importance.

The graft below shows the renewable production for the 3. and 4. of april 2019. The blue line is the actual production in a 5 minute resolution and the orange is the mean hourly production.

![](figures/april-production-hour.png) 

The graft visualizes the volatility of renewable production, within 12 hours the production fell from 2.500 MWh to 750 MWh or in other words, fell to 30%. 

Hourly resolution was chosen since it is what we currently have available for all of measurements points in Denmark through [the DataHub](datahub.md) and it being the market resolution.

The Market resolution (in Denmark) is currently hourly, but it is work in progress to move it to a lower resolution in the future (15 or 5 minute)

To enable higher resolution in the future, each GGO has both a **begin** and **end** date-time. This way, when higher resolutions (15 min) becomes available, four GGOs are issued instead of a single for each hour, with the corresponding begin and end time.

The dates are in ISO-8601 format, the begin is inclusive and the end exclusive. Example: 

    "begin": "2020-07-14T10:00:00Z"
    "end": "2020-07-14T11:00:00Z"

This will be interpreted as a single GGO for 1 hour where the next one will begin at the end of the previous one.

## GGO size (watt hour)

We want to be as granular as our data will allow, and since we measure all meters in watt-hour, this is also what we will issue all GGOs on. 

The amount on each GGO is therefore in watt-hour and is an whole integer number.

Example: a GGO containing 12752 watt-hour

    "amount": 12752


## GGO sector

For geographical correlation we have added a sector to each GGO. 

Currently these sectors are defined as the price-area, in Denmark DK1 and DK2.

But this could also be at a more granular level if there are congestion within a single price-area. 

This is why the word **sector** was chosen as not to predefine the limit.

Example:

    "sector": "DK1"

## GGO technology and fuel code

Based on the work from <a href="https://www.aib-net.org/sites/default/files/assets/eecs/facts-sheets/AIB-2019-EECSFS-05%20EECS%20Rules%20Fact%20Sheet%2005%20-%20Types%20of%20Energy%20Inputs%20and%20Technologies%20-%20Release%207.7%20v5.pdf">AIB (Association of Issuing Bodies) fact 5</a> on technology and fuel codes, we use their codes as used in the GO system on the GGOs.

Their tech and fuel codes are an already established standard and is a verbose way to describe the origin of the electricity.

Each single GGO has a tech_type and fuel_type field appended to them:

Example: Offshore wind power.

    "tech_type": "T020002"
    "fuel_type": "F01050100"

## GGO Origin

Each GGO has a origin reference. This reference can be both to the origin measurement, it was issued from or a reference to the GGO it was split from.

# GGO - actions