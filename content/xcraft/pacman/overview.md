---
title: 'Overview'
date: 2020-07-24
weight: 10
tags: ['toolchain']
draft: true
---

> TODO: some steps are not implemented...

The packages are created with wpkg as presented in the following diagram.

![Overview diagram](/img/pacman.overview.svg)

The definitions for all packages are in the `packages/` directory.

### Step 1

All packages are described by a `config.yaml` file, see
[The package definitions](package.def.md) page for details. Only the YAML file
is mandatory. But for some packages, it can be necessary to have some resources
directly in the package definition directory (only for core packages or very
small packages). See [... TODO: page unavailable ...](TODO) for details.

{{% notice info %}} When a package must be created, the `config.yaml` file is
used in order to generate the `WPKG/control` file that wpkg look for. This
control file is based on the Debian rules, where some fields are just copied
from the `config.yaml` file. The step from 1 to 2 is not just a file conversion.
A mechanism looks for the resource URI and the embedded flag. If necessary, the
resources are copied (downloaded) along the `WPKG` directory. {{% /notice %}}

### Step 2

The `WPKG` directory contents files dedicated to wpkg. This step is temporary,
then the files are located to `var/tmp/wpkg/`. Many files can exist around the
control file. All other resources are located in the parent directory
(`WPKG/..`). See [... TODO: page unavailable ...](TODO) for details.

According to the architecture field in the `control` file, two types of package
can be created. The most common type is the binary packages, and the second type
is the source packages.

### Step 3

- In the case of binary architecture, a binary package is created by wpkg, then
  it is stored in a repository.

- In the case of source architecture, a source package is created by wpkg, then
  it is stored in the same repository that for the binary packages.

There are two repositories. The packages used only for development and the
packages for the consumer.

### Step 4

One repository is located in `var/wpkg/`. The wpkg command creates a index file
along the deb packages. This repository is only for development packages.

{{% notice info %}} When a package is in a repository, it can be installed
easily in a target with its dependencies. Some packages can be stored directly
in the production repository if necessary. {{% /notice %}}

### Step 5

The development packages (step 4) are deployed in a `devroot/`. This location
concerns only the tools (for the toolchain) and the source packages.

{{% notice info %}} The goal for installing source packages in `devroot/` is to
compile these products in order to generate binary packages. Then these binary
packages are stored in the production repository. {{% /notice %}}

### Step 6

The production repository can be anywhere. This is the main repository for the
consumers.

{{% notice note %}} At this point, we can use the production repository in any
targets. For example it can be used in order to install the products in a
virtual machine. {{% /notice %}}

### Step 7

It's the target location for the final products.
