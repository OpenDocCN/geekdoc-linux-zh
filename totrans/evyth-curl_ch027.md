# Linux

Linux distributions come with packager managers that let you install software that they offer. Most Linux distributions offer curl and libcurl to be installed if they are not installed by default.

## Ubuntu and Debian

`apt` is a tool to install prebuilt packages on Debian Linux and Ubuntu Linux distributions and derivatives.

To install the curl command-line tool, you usually just

[PRE0]

…and that then makes sure the dependencies are installed and usually libcurl is then also installed as an individual package.

If you want to build applications against libcurl, you need a development package installed to get the include headers and some additional documentation, etc. You can then select a libcurl with the TLS backend you prefer:

[PRE1]

or

[PRE2]

## Redhat and CentOS

With Redhat Linux and CentOS Linux derivatives, you use `yum` to install packages. Install the command-line tool with:

[PRE3]

You install the libcurl development package (with include files and some docs, etc.) with this:

[PRE4]

## Fedora

Fedora Workstation and other Fedora based distributions use `dnf` to install packages.

Install the command-line tool with:

[PRE5]

To install the libcurl development package you run:

[PRE6]

## Immutable Fedora distributions

Distributions such as Silverblue, Kinoite, Sericea, Onyx, … use `rpm-ostree` to install packages. Remember to restart the system after install.

[PRE7]

To install the libcurl development package you run:

[PRE8]

## nix

[Nix](https://nixos.org/nix/) is a package manager default to the NixOS distribution, but it can also be used on any Linux distribution.

In order to install command-line tool:

[PRE9]

## Arch Linux

curl is located in the core repository of Arch Linux. This means it should be installed automatically if you follow the normal installation procedure.

If curl is not installed, Arch Linux uses `pacman` to install packages:

[PRE10]

## SUSE and openSUSE

With SUSE Linux and openSUSE Linux you use `zypper` to install packages. To install the curl command-line utility:

[PRE11]

In order to install the libcurl development package you run:

[PRE12]

### SUSE SLE Micro and openSUSE MicroOS

These versions of SUSE/openSUSE Linux are immutable OSes and have a read only root file system, to install packages you would use `transactional-update` instead of `zypper`. To install the curl command-line utility:

[PRE13]

And to install the libcurl development package:

[PRE14]

## Gentoo

This package installs the tool, libcurl, headers and pkg-config files etc

[PRE15]

## Void Linux

With Void Linux you use `xbps-install` to install packages. To install the curl command-line utility:

[PRE16]

In order to install the libcurl development package:

[PRE17]