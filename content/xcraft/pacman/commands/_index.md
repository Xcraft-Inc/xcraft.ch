---
title: 'Commands'
date: 2020-07-24
weight: 20
draft: true
---

Primary module that drives the `wpkg` command in order to make, build, install,
remove, etc,... packages.

## pacman.list

List all packages available in the `packages/` directory.

## pacman.edit

Create or edit a package [definition][2]. This function will start a wizard and
generates a new directory in the `packages/` directory. The [definition][2] is
stored in a `.yaml` file.

## pacman.make

Generate a new package `.deb` accordingly to the [definitions][2]. This package
is saved in a local [repository][3].

## pacman.install

Install a package from the [repository][3] in the [`devroot/`][1]. The list of
available repositories depends of the current [`devroot/`][1] settings.

## pacman.reinstall

Unlike the install command, here it's possible to reinstall an already installed
package. Excepted for this case, the behaviors are the same that the install
command.

## pacman.status

It's possible to check if a package is already installed with this command.

## pacman.build

When a package is a _source_ package, then it's possible to use this command in
order to generate a binary package. Then all build steps are handled here. The
new package is published in the repository. Then it's possible to install this
new package in `devroot/`.

Many steps are necessary in order to build a binary package.

## pacman.remove

When a package was installed in `devroot/`, this command provides the
possibility to uninstall.

## pacman.clean

The make command generates temporary files in `var/tmp/wpkg/`. Here, it's
possible to remove these files. The make command is already using the clean
command internally.
