OgreAL Perl bindings
====================

This distribution provides Perl bindings for the OgreAL C++ 3D audio library
by Casey Borders. See http://ogreal.sourceforge.net/ or search for
OgreAL on the OGRE 3D wiki http://www.ogre3d.org/wiki/index.php .

OgreAL integrates the OpenAL audio library with OGRE 3D. That lets you
attach a sound to a node in a 3D scene, so that the sound seems to be
coming from a certain place within the scene, which could be interesting
in games for example.

I was going to just bundle this in with the Perl Ogre module, but putting it
in a separate module allows people to download it only if they need it
(Perl Ogre is getting pretty big already, as it is...)


DEPENDENCIES

Required:

  Ogre

In order to use this module, you'll need to install the Ogre Perl module.

  OgreAL
  libopenal

You also have to have recent (svn) versions of these two libraries.
There are installation notes below.


Optional:

  OIS
  Ogre::BetaGUI

OIS provides a way of obtaining user input. If you have another way to do
that (SDL, for example), or your application doesn't accept user input,
then you don't have to install OIS.

Ogre::BetaGUI allows you to make a simple graphical user interface
with a mouse cursor, windows, buttons, text boxes, and labels.
It is required for running examples/doppler.pl but otherwise
it's up to you.


INSTALLATION

Installation is unfortunately probably going to be difficult
unless you're at ease compiling things from svn repositories.
It would be almost miraculous if you managed to install it
under Windows; I look forward to receiving your patches, though.
(OGRE 3D and OgreAL themselves are cross-platform, but I only
use Ubuntu (GNU/Debian/Linux), and I don't know how to build them
as Perl modules on Windows, or Mac for that matter.)

Here are quick notes on how to install OgreAL.
Note that these show how to install locally, not system wide
(basically remove --prefix and do `make install` as root).


# Install OGRE 3D, etc., as you already have for Perl Ogre.
Should be able to get headers/includes with `pkg-config --cflags OGRE`,
`pkg-config --libs OGRE` (at least on Ubuntu).


# Install OIS 1.0 (important: not 0.99+1.0rc1)
Download from sourceforge, then:
$ tar -zxf ois-1.0.tar.gz
$ cd ois/
$ ./bootstrap
$ ./configure --prefix=$HOME/.ois-install
$ make
$ make install
Env vars for OgreAL (unnecessary):
$ export OIS_CFLAGS="-I$HOME/.ois-install/include"
$ export OIS_LIBS="-L$HOME/.ois-install/lib -lOIS"


# Install OpenAL and ALUT from svn
$ svn checkout http://www.openal.org/repos/openal/trunk openal
$ cd openal/OpenAL-Sample/     # different for MacOSX and Windows
$ ./autogen.sh
[bunch of warnings, bleh]
$ ./configure --prefix=$HOME/.openal-install
$ make
$ make install
# OgreAL deprecated ALUT, but make sure it's the recent version just in case
$ cd ../alut/
$ ./autogen.sh
[bunch of warnings, bleh]
$ ./configure --prefix=$HOME/.openal-install      # same directory
$ make
$ make install
Env vars for OgreAL (unnecessary):
$ export OPENAL_CFLAGS="-I$HOME/.openal-install/include"
$ export OPENAL_LIBS="-L$HOME/.openal-install/lib -lopenal -lalut"

Note: on my system (Ubuntu) I had to add a file ~/.openalrc
with the following in it:

(define devices '(alsa dsp arts esd native null))

Your mileage may vary.


# Make sure OgreAL sees the right pkg-config files, just in case
# (this part is necessary if you didn't set the above env vars)
$ export PKG_CONFIG_PATH=$HOME/.ois-install/lib/pkgconfig:$PKG_CONFIG_PATH
$ export PKG_CONFIG_PATH=$HOME/.openal-install/lib/pkgconfig:$PKG_CONFIG_PATH


# Install OgreAL (0.2.5 as of 14 Dec 2007)
$ svn co https://ogreal.svn.sourceforge.net/svnroot/ogreal/trunk/OgreAL-Eihort
$ cd OgreAL-Eihort/
$ ./bootstrap
$ ./configure --prefix=$HOME/.ogreal-install
$ make
$ make install

Try some demos:
$ cd Demos/bin/
$ ./Basic

Note: Linux support is still being worked on. See the section on OgreAL
in the ogre3d wiki.


Now that OgreAL is installed, we can try the Perl module.
If you followed the above instructions, OgreAL installed a .pc file
that can make things easier. Just do this:

$ export PKG_CONFIG_PATH=$HOME/.ogreal-install/lib/pkgconfig:$PKG_CONFIG_PATH

then pkg-config can find the C++ headers and libraries.
Otherwise, set these environment variables:

 OGREAL_CFLAGS
 OGREAL_LIBS

Set them to what? Uhhh...?

Then finally build the Perl module normally:

   perl Makefile.PL
   make
   make test
   make install

Note: you might be asked questions during the install. It is
VERY IMPORTANT that you have either the PKG_CONFIG_PATH
or all of the following environment variables set before building:
OGREAL_CFLAGS, OGREAL_LIBS, OPENAL_CFLAGS, OPENAL_LIBS.
Failure to do that correctly may mean that Ogre::AL gets linked
to older versions, particularly of libopenal; this will cause
frustrating and confusing errors.

You might have to edit Makefile.PL to get it to work for your system.
If so, please let me know.


BUGS

No doubt... Please report any bugs/suggestions to <slanning@cpan.org>.


COPYRIGHT AND LICENCE

Copyright 2007, Scott Lanning. All rights reserved.

This program is free software; you can redistribute it and/or modify
it under the same terms as Perl itself.


OgreAL is copyright Casey Borders and is currently (14 Dec 2007)
distributed under the LGPL license. See the OgreAL distribution
for more details.
