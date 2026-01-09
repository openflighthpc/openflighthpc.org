---
title: Understanding the CLI
description: A breakdown of what a CLI prompt could look like
weight: 2
---
Usually a lot of HPC environment usage takes place via the Linux command line. For unfamiliar users here is a brief description of a typical CLI prompt.

```bash
[flight@chead1 folder]$ echo Hello World!
Hello World!
```

- `flight` is the name of the user.
- `chead1` is the node the user is currently on.
- `folder` is the directory the user is currently in. If this is a `~` then that means you are in your home directory.
- `$` indicates whether the user is a root user or normal user (root users see a `#`).
- `echo Hello World!` is a command that prints out "Hello World!". This is where commands you type will go.

> The prompt can vary based on user preference and global environment configuration. The above is an example for illustrative purposes
