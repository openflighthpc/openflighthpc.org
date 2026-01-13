---
title: Copying Data Between Hosts
description: How to use the SCP command to copy data
weight: 3
---
In a HPC environment there is likely to be a [shared filesystem]({{< relref "knowledge/hpc-environment-basics/working-with-data/storage-types.md" >}}#shared-storage), so it is not normally necessary to copy data directly between nodes in the research environment. Users simply need to place the data to be shared in their home-directory on the login node, and it will be available on all compute nodes in the same location.

If necessary, users can use the `scp` command to copy files between nodes. For example, the following will copy the file `myfile.txt` stored under `/tmp/` on `node01` to the `cwd` (`.`) of this host:

```bash
scp node01:/tmp/myfile.txt .
```

Alternatively, users could login to a host that has unshared data on and copy the data back to the shared filesystem.
