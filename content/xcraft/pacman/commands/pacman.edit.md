---
title: 'pacman.edit'
date: 2020-07-24
weight: 15
tags: ['toolchain', 'pacman']
draft: true
---

## The package definitions

The package definitions are located in the `packages/` directory where you can
find the sub-directories of the projects. Every project has a file named
`config.yaml`. This file can be edited by hand or with the `zog` command.

{{% notice tip %}} It's better to use `zog` or `lokthar` for editing unless you
know exactly what are you going. {{% /notice %}}

### Create or modify a package definition

The command is just: `zog create <the package name>`. Then a prompt will ask for
some questions about this new package.

#### Package name (required)

The name must follows these rules:

Must consist only of lower case letters (a-z), digits (0-9), plus (+) and minus
(-) signs, and periods (.). They must be at least two characters long and must
start with an alphanumeric character.

#### Version (required)

The version must follows these rules:

> http://windowspackager.org/documentation/implementation-details/debian-version

#### Maintainer's name (required)

It is commonly the first name and the last name.

#### Maintainer's email (required)

Only public domain should be used for the email.

#### Host architecture (required)

You can choose all operating systems and CPU architecture where the software is
supported. Note that a package will be built for each architecture. If the
package has not dependency on an architecture (like the documentation for
example) you must use 'all'. Of course, all package for sources must use the
'source' architecture.

#### Brief description (required)

At least this description is required and must be not too long (max 70
characters).

#### Long description

The brief description must not be repeated here.

#### Add a dependency

You can add a dependency if required. The list of dependencies is the list of
packages available in the `packages/` directory. For example, you must add a
dependency on `libfoobar` where the version range is (>= 1.0 and < 2.0). In this
case you must add two dependencies.

1. Add `libfoobar`
2. Set the range: `>= 1.0`
3. Add `libfoobar` a second time
4. Set the range: `<< 2.0`

The range is optional. You can return immediately if you have no restriction on
the version.

#### config.yaml

A `config.yaml` file is created in your `libfoobar` directory. You can open this
file with your preferred text editor.

For example:

```yaml
name: libfoobar
version: 0.1.0
maintainer:
  name: John Doe
  email: 'john@doe.com'
architecture:
  - win32-i386
  - linux-i386
  - darwin-i386
description:
  brief: The foobar library for testing.
  long: It is dedicated to a test for a package definition file.
dependency:
  libfoobar2:
    - '>= 1.0'
    - << 2.0
distribution: toolchain/
```
