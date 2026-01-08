---
title: Carbon Leaderboard
description: A website for collecting & comparing carbon impact of physical & virtual servers
date: 2024-05-01
toc: true
params:
  status: archived
  source: https://github.com/openflighthpc/carbon-leaderboard
archetype: lab
---

## Overview 

The OpenFlight Carbon Leaderboard was an initiative aiming to provide a simple methodology and clear comparison of Carbon Impact Estimates for a range of servers while adding a light competitive edge. The service helped get an understanding of the impact of resources (both physical and virtual) at various levels of load. 

![](/images/labs/carbon-leaderboard.png)

To deal with HPC services where multiple systems may have identical specifications the leaderboard groups them. These groups then have some helpful (and somewhat playful) comparisons for their emissions

![](/images/labs/carbon-leaderboard-asset.png)

## Client Carbon Data

To gather the data a [helper script](https://github.com/openflighthpc/carbon-leaderboard/tree/main/carbon-client) is provided which collates anonymous server data, allows for user input of some data points which affect estimates (e.g. location) and submit that data to the leaderboard.

The project utilises the [Boavizta API](https://doc.api.boavizta.org/) for performing calculations based off of system specifications and collected carbon
impact reports (from manufacturers).

This project uses the _Global Warming Potential (GWP)_ metric to identify the
_Carbon Equivalent_ of the system specifications at run time (this means that
estimations of impact of hardware manufacture are **not** included in the value).
This value is provided in grams of CO2 equivalent per hour (gCO2eq/hr).

## How Are Virtual Devices Estimated?

The estimation of virtual devices is still in early estimation and will improve over time. One of the main complexities in estimating the impact of virtual resources is through knowing the hardware that hosts the virtual system.

Some initial understanding of the hardware backing AWS instances is implemented in the underlying API which allows for a better estimation in the impact of those resources.

For other platforms (e.g. Azure, GCP, OpenStack), the underlying hardware is not known or is bespoke as is the case with private cloud solutions, such as OpenStack.

For example, it has been observed that estimations for virtual resources on OpenStack have seemed higher than expected because OpenStack will, by default, provide very little CPU layout information to instances. This leads to each core that a VM has being seen as a separate physical CPU. With more physical CPUs this will largely inflate the carbon estimates for the virtual system. It is possible to address this issue on an OpenStack deployment by defining the CPU topology.

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

