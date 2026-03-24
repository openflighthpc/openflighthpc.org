---
title: Flight Solo
description: HPC-ready, platform-agnostic image approach to deploying HPC resources
date: 2024-08-01
params:
  status: archived
  source: https://github.com/openflighthpc/docs.openflighthpc.org/tree/release/2024.1/docs/docs/flight-solo
toc: true
---

## What is Flight Solo?

Flight Solo is an HPC-ready, platform-agnostic image approach to deploying HPC resources.

Flight Solo provides a personal High Performance Computing (HPC) environment for research and scientific computing. Compatible with on-demand, reserved and spot instances, Flight Solo rapidly delivers a whole HPC software environment, ready to go and complete with job scheduler, applications and remote desktop service. Multiple software application frameworks are available for use including SPack, Easybuild and Conda containing many hundreds of different programs, libraries, compilers and MPIs. Data management tools for POSIX and S3 object storage are also included to help users transfer files and manage storage resources.

Built on EL9, loaded with the [Flight Environment]({{< relref "labs/flight-environment" >}}) and presented with useful cloud-init integrations - the Flight Solo image provides an image suitable for:

- Researchers & End-Users to get going with HPC, fast
- Rapid deployment of replicable HPC environments
- Proof-of-concept cloud HPC environments
- Evaluating the Flight Environment

## Understanding Flight Solo

Flight Solo is a combination of tools, process and knowledge with the aim of providing a streamlined solution to launching HPC systems. It broadly consists of:

- HPC-Optimised Rocky 9 OS
- Cloud-Init Integration
- Flight Environment

### HPC Optimised Rocky 9 OS

Flight Solo is built of a clean Rocky EL9 image. This image has been tweaked to be ready to do HPC through:

- Kernel drivers set to support most virtualisation platforms
- Suitable security settings

### Cloud-Init Integration

[Cloud-Init](https://cloudinit.readthedocs.io/en/latest/index.html) provides the ability to inject runtime information and configuration into the OS of cloud systems. It can be used to perform many different setup tasks, detailing this setup is outside the scope of this documentation. Information

### Flight Environment

Flight Solo comes with the complete [Flight Environment]({{< relref "labs/flight-environment.md" >}}) installed by default along with suitable configuration modifications to get the most out of the tools from the get-go.

## Platform Agnostic

The image build process for Flight Solo empowered a platform-agnostic distribution. The image would function the same on OpenStack, AWS and Azure. It was also available through the AWS Marketplace. 
