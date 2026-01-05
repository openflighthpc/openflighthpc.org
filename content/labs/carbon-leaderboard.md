---
title: Carbon Leaderboard
date: 2024-05-01
params:
  status: archived
  source: https://github.com/openflighthpc/carbon-leaderboard
archetype: lab
---
The OpenFlight Carbon Leaderboard was an initiative aiming to provide a simple methodology and clear comparison of Carbon Impact Estimates for a range of servers while adding a light competitive edge. The service helped get an understanding of the impact of resources (both physical and virtual) at various levels of load. 

![](/images/labs/carbon-leaderboard.png)

To deal with HPC systems where multiple systems may have identical specifications the leaderboard groups identical items. These groups then have some helpful (and somewhat playful) comparisons for their emissions

![](/images/labs/carbon-leaderboard-asset.png)

## Client Carbon Data

To gather the data a [helper script](https://github.com/openflighthpc/carbon-leaderboard/tree/main/carbon-client) is provided which collates anonymous server data, allows for user input of some data points which affect estimates (e.g. location) and submit that data to the leaderboard.

The project utilises the [Boavizta API](https://doc.api.boavizta.org/) for performing calculations based off of system specifications and collected carbon
impact reports (from manufacturers).

This project uses the _Global Warming Potential (GWP)_ metric to identify the
_Carbon Equivalent_ of the system specifications at run time (this means that
estimations of impact of hardware manufacture are **not** included in the value).
This value is provided in grams of CO2 equivalent per hour (gCO2eq/hr).

## Are Values Accurate? 

Calculating carbon equivalent impacts is not a simple procedure and takes in multiple factors:

- Manufacturer's reports on hardware
- Cleanliness of the power to a country
- Available information on the reported hardware

Therefore, the values are merely estimates to the best of the ability of the Boavizta service (which has put considerable effort and transparency into their methodology), which can be read about on their website .

Things that are not currently considered by the leaderboard:

- The cleanliness of the power source to the data-centre housing the servers
- Impact of the manufacture of the hardware
- Any initiatives taken to offset carbon

