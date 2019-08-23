# sports

**WIP**

Sports is a package manager for 9front intended to exist side by side with the existing ports tree. Sports is primarily inspired by the Crux Linux distribution ports system and the Slackware Linux Slackbuilds system. 

The name sports is an adjustment of ports while the 9front ports tree continues to exist: https://code.9front.org/hg/ports

## Installation

The recommended directory for installation is `/sys/ports`. 

Clone:

	% git clone https://github.com/henesy/sports

Sports will install ports to `$ports/root`. As such, you should bind this directory over root in some form. The sports management scripts are contained in `$ports/bin`. 

For a user-specific installation, you could add the following to your `$home/profile`:

	# Ports tree additions
	portsroot=$home/ports
	bind -a $ports/bin /
	bind -a $ports/root /

## Usage

Update the ports tree:

	% prt-pull

Install a port from somewhere outside the ports tree:

	% prt-inst games/breakout

Install a port from a port's directory:

	% cd $ports/games/breakout
	% prt-inst

Delete a port from somewhere outside the ports tree:

	% prt-del games/breakout

Delete a port from a port's directory: 

	% cd $ports/games/breakout
	% prt-del

Get info about a port:

	% prt-info games/breakout

Explicitly build a port from outside the ports tree:

	% prt-mk games/breakout

Explicitly build a port from the port's directory:

	% cd $ports/games/breakout
	% prt-mk

Search for text in a port:

	% prt-grep breakout
	...
	%

## Writing ports

Each port lives in a directory under the `$ports` root directory as per `$ports/category-subcategory/portname`. 

Every port directory has a `prtfile` which is ingested by the `prt-*` scripts. 

A port may also contain a `patches/` directory which may be ingested by a `patch` function in the `prtfile` as described later. 

Functions are called from the `$workroot` directory of the port. 

### Required headers

Description -- One-line description of the port.

Home -- The homepage, or equivalent, for a port
	- 9p server directories should be dialstring^path such as `tcp!9pio/foo/bar`.

Maintainer -- The port maintainer, if you are submitting a new port, this is you.

### Required variables

name -- The name of the port to install. 

version -- For archive or 9p type ports, this is should be the release version. For git or hg type ports, this should be the commit hash of the target version to checkout upon cloning time. 

source -- The URL or dialstring for the port's source code. 

type -- Type of port source, valid options are: archive, git, hg, 9p. This may be removed in the future. 

workroot -- The directory, relative to the port's directory, in which the build, etc. functions should be called. 

### Optional variables

depends -- Should contain a valid rc(1) list of all ports which must be installed for this port to be installed. 

notice -- An optional message to provide to the user after `install()` is called. 

### Required functions

build -- Should fully build the port from source. Note that you'll be cd(1)'d into `$workroot` before this is called. 

install -- Should install the port to `$ports/root` in a form consistent with the Plan 9 filesystem layout. 

uninstall -- Should completely uninstall the port from `$ports/root` in a manner which affects only the port currently being uninstalled, so no wildcards, etc. 

### Optional functions

clean -- This function is called after `install()` and should clean unnecessary files out of the work directory. 

patch -- This function is called before `build()` and should be used if a `patches/` directory exists. 

## Testing ports

You can do a full install of the ports tree to evaluate if the builds are working with:

	% prt-inst *

## Contributing

Please do.
