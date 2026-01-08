---
title: GHPC (Diskless Stack)
description: A diskless PXE boot HPC stack with overlayfs & cloud-init
date: 2025-12-01
params:
  status: active
  source: ghpc.openflighthpc.org
toc: true
---

![](/images/labs/ghpc-how-it-works.gif)

## Overview

GHPC is an experimental Rocky 9 stack built around diskless PXE booting and overlayfs to provide a scalable, manageable network of HPC clusters to delivery the HPC service for a single site. 

GHPC is shipped as 2 VM appliances that you can deploy on your chosen platform (e.g. libvirt, public cloud). These appliances provide the tools needed to create, deploy and manage multiple HPC & AI stacks ("clusters"). The Director & Domain sit in a "Domain Network", the director creates Controllers which, in turn, manage services within a cluster.

The goal of this project was to provide a quick-and-easy way of getting an example HPC stack running while also being reflective of what a production HPC stack looks like in Alces deployments. It also experiments with disaster recovery and change management processes - when a node is rebooted it's original state will be restored unless changes are made to the source which allows for experimentation without risk of permanently breaking the host OS.

## Architecture

### Diskless Exports 

There are 4 different diskless images that systems can boot from:
- Hunter: If they don't have any assigned identity then they go into the hunter image, this image is used to identify system information (like boot interface and MAC address)
- Controller: The identity for a controller node, containing the SLURM controller daemon amongst other cluster services
- Login: The identity for a login node, containing Flight Web Suite for easy remote access
- Compute: The identity of a compute node, a basic installation prepared for running HPC workloads


Additionally, each cluster has a "shared persistent" storage, this allows for layering and sharing of files that need to match across all systems. For example, the `/etc/slurm/slurm.conf` file is served from here so it only needs to be changed here for all nodes in the cluster to have a matching configuration file. 

Beyond the "shared persistent" for the cluster, each system itself gets a "persistent" export for node-specific changes to be made in.

Due to the diskless nature it should be possible to change host source images to be different configurations (or even OSes) without painful rebuilds of multiple systems.

### Cloud Init

The personality information (such as hostname, network IPs, interface names, etc) is injected into the host at boot-time via cloud-init.

### Overlayfs

While the shared image is mounted read-only to the host, overlayfs is used to provide a writeable layer over the top so hosts can be modified live. 

A benefit of using overlayfs in this way is that it means where a host diverges from the source image can be found and referenced in `/run/upper/data/HOST`. 

## Documentation

Documentation for this project is kept minimal on the project site and is, instead, mainly shipped within the Director image itself. This ensures it stays in sync and empowers a fully offline setup. 

The [Glow](https://github.com/charmbracelet/glow) project is used in conjunction with [a simple script](https://github.com/alces-flight/ghpc-docs) to render markdown in a legible and easily navigable manner.

![](/images/labs/ghpc-docs.gif)
