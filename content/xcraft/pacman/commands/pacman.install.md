---
title: 'pacman.install'
date: 2020-07-24
weight: 30
tags: ['toolchain', 'pacman']
---

If the package is already installed, nothing happens. If the currently installed
package is older, then this action will upgrade with the new version.

Of course, all dependencies will be installed too. If a required dependency is
not found in the repository, the installation will fail.

### MS Windows and MSI

Maybe you have a package with an _MSI_ installer. In this case, it can be
possible that this _MSI_ needs a reboot in order to continue the installation.
If it happens, you must restart the system, then you can send the same command
again, then the next dependencies will be installed as expected.

{{% notice note %}} Note that the detection about a reboot necessity is still
WIP. {{% /notice %}}

See [xcraft-core-process][5] for more informations.
