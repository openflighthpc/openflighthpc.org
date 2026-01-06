---
title: GHPC (Diskless Stack)
date: 2025-12-01
params:
  status: active
  source: ghpc.openflighthpc.org
---

![](/images/labs/ghpc-how-it-works.gif)

GHPC is an experimental stack built around diskless PXE booting and overlayfs to provide a scalable, manageable network of HPC clusters to delivery the HPC service for a single site. 

GHPC is shipped as 2 VM appliances that you can deploy on your chosen platform (e.g. libvirt, public cloud). These appliances provide the tools needed to create, deploy and manage multiple HPC & AI stacks ("clusters"). The Director & Domain sit in a "Domain Network", the director creates Controllers which, in turn, manage services within a cluster.

The goal of this project was to provide a quick-and-easy way of getting an example HPC stack running while also being reflective of what a production HPC stack looks like in Alces deployments.
