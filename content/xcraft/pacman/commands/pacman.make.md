---
title: 'pacman.make'
date: 2020-07-24
weight: 20
tags: ['toolchain', 'pacman']
---

It is possible to pass several packages or nothing. In the first case, all
packages will be generated. If nothing is passed, then all packages are
considered.

```sh
pacman.make                     # To make all packages.
pacman.make package1,package2   # To make only two packages.
```

Sometimes it is interesting to make a package will all its dependencies. The
following pattern `@deps` will do the trick. Of course, you can add other
packages.

```sh
pacman.make package1,@deps             # To make package1 and its dependencies.
pacman.make package1,@deps,package2    # To make package1, its dependencies and
                                       # the package2.
```

In some cases (like for Continuous Integration), you can overload some
properties for a specific package or all packages. The overload is done on the
properties available in the definition files.

```sh
pacman.make package1 \                # Overload the version of package1.
            p:version=1.0.0
pacman.make package1,package2 \       # Overload only the version of package1.
            p:package1:version=1.0.0  # In the case of package2, it will use
                                      # its own version.
```

It is possible to overload all packages with only one `p:` argument.

```sh
pacman.make package1,package2 \   # Overload the distribution globally.
            p:distribution=test/
pacman.make p:distribution=test/  # Like previous with all packages.
```

An other example very useful with a CI (when a tag is pushed for example).

```sh
pacman.make srcpackage,@deps \
            p:srcpackage:version=1.0.0 \
            p:srcpackage:data.get.ref=v1.0.0
```

It makes the srcpackage and its dependencies. The package will be created with
the version 1.0.0, and because this package is related to a git repository, the
tag `v1.0.0` will be used.

{{% notice info %}} In this case, the reference can be a commit, a branch or a
tag. {{% /notice %}}

### Timestamps

A package is not re-maked if the files in its `packages/` have not changed. A
timestamp is saved in the `var/xcraft-contrib-pacman/` directory with the
current timestamp. Before a `make`, this timestamp is compared against all
timestamps (mtime) of the files available in `packages/`.

{{% notice note %}} Note that if a `data.get.uri` target has changed, this
mechanism will not detect anything. File, repository, etc,... is only downloaded
or copied by the [xcraft-contrib-peon][4] (_make_ or _install_ time, it depends
if the package embeds the data or not).

{{% /notice %}}

Here an example where it can be a problem:

```yaml
# Part a y YAML definition file.
data:
  get:
    uri: home:///foobar
```

The `foobar/` directory is located in the toolchain, but for _pacman_, it is
just a data location like `http` for example. If you change something in the
`foobar/` directory, nothing will be detected by pacman.

{{% notice note %}} Note that the version should probably change in this case.
Then the package will be processed because the definition file will have a newer
timestamp. {{% /notice %}}
