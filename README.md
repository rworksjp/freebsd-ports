# freebsd-ports
[![License](https://img.shields.io/github/license/rworksjp/freebsd-ports.svg?maxAge=2592000)](https://tldrlegal.com/license/bsd-2-clause-license-(freebsd))

The add-on ports tree for FreeBSD ports system.

## How to use

To build packages in this repository:

- Merge [official FreeBSD ports tree](https://github.com/freebsd/freebsd-ports.git) and [this repository](https://github.com/rworksjp/freebsd-ports.git) with [portshaker](https://github.com/smortex/portshaker)
- Build package from merged ports tree with [poudriere](https://github.com/freebsd/poudriere)

### Preparation

Install `portshaker` and `poudriere`:

```console
# pkg install portshaker poudriere
```

(Recommended but optional) Install `dialog4ports`, to specify ports build option with `poudriere options`:

```console
# pkg install dialog4ports
```

Put `/usr/local/etc/portshaker.d/freebsd_ports`:

```sh
#!/bin/sh
# $Id$

. /usr/local/share/portshaker/portshaker.subr

method="git"
git_clone_uri="https://github.com/freebsd/freebsd-ports.git"

run_portshaker_command $*
```

and `/usr/local/etc/portshaker.d/rworksjp`:

```sh
#!/bin/sh

. /usr/local/share/portshaker/portshaker.subr

method="git"
git_clone_uri="https://github.com/rworksjp/freebsd-ports.git"

run_portshaker_command $*
```

Edit `/usr/local/etc/portshaker.conf`:

```sh
mirror_base_dir=/var/cache/portshaker
poudriere_ports_mountpoint="/usr/local/poudriere/ports"

rworksjp_poudriere_tree=rworksjp
rworksjp_merge_from="freebsd_ports rworksjp"
```

(Optional) When you have a ZFS pool and want to build packages with it, the following instructions are required:

- Add `ZPOOL=zroot` line in `/usr/local/etc/poudriere.conf`, if you have a ZFS pool named `zroot`.
- Create a ZFS filesystem for portshaker, `zfs create -o mountpoint=/var/cache/portshaker zroot/portshaker`, if you have a ZFS pool named `zroot`.
- Add `use_zfs=yes` and `poudriere_dataset="data/poudriere"` lines in `/usr/local/etc/portshaker.conf`.

For people building lots of packages, compression option of ZFS filesystem give a help to reduce disk usage.

Creating poudirere builder jail, for example:

```console
# poudriere jails -c -j FreeBSD:10:amd64 -v 10.2-RELEASE -a amd64
```

### Updating and building ports tree

Update ports trees and merge them with `portshaker`, instead of `poudriere ports -u`:

```console
# portshaker
```

Build `sysutils/glusterfs` package with `FreeBSD:10:amd64` builder:

```console
# poudriere bulk -j FreeBSD:10:amd64 -p rworksjp sysutils/glusterfs
```

This creates packages, `glusterfs` its own and required, usually under `/usr/local/poudriere/data/packages/FreeBSD:10:amd64-rworksjp/`

## License

Licensed under the BSD 2-clause License. For detailed information see [LICENSE](./LICENSE).

## Further readings

About [portshaker](https://github.com/smortex/portshaker), consult following:

- [`man 8 portshaker`](https://www.freebsd.org/cgi/man.cgi?query=portshaker&apropos=0&sektion=8&manpath=FreeBSD+10.2-RELEASE+and+Ports&arch=default&format=html): how to use portshaker command
- [`man 5 portshaker.conf`](https://www.freebsd.org/cgi/man.cgi?query=portshaker.conf&sektion=5&apropos=0&manpath=FreeBSD+10.2-RELEASE+and+Ports):configuration formats and options
- [`man 5 portshaker.d`](https://www.freebsd.org/cgi/man.cgi?query=portshaker.d&sektion=5&apropos=0&manpath=FreeBSD+10.2-RELEASE+and+Ports): how to create or change files under /usr/local/etc/portshaker.d/
- [smortex/portshaker](https://github.com/smortex/portshaker): portshaker source code on github, if you want to follow how portshaker works

About [poudriere](https://github.com/freebsd/poudriere), consult following:

- [Poudriere section in FreeBSD Porter's Handbook](https://www.freebsd.org/doc/en/books/porters-handbook/testing-poudriere.html): general usage to building ports with poudriere
- [poudriere documentation](https://github.com/freebsd/poudriere/wiki): poudriere official documentation, including design and combination with other tools
- [`man 8 poudriere`](https://www.freebsd.org/cgi/man.cgi?query=poudriere&apropos=0&sektion=8&manpath=FreeBSD+10.2-RELEASE+and+Ports&arch=default&format=html): about poudriere command and subcommand and how to use these
- [freebsd/poudriere](https://github.com/freebsd/poudriere/): poudirere source code on github, if you want to follow how poudriere works
