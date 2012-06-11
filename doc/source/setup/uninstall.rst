Uninstall
=========

To remove Mango from your system, follow the steps appropriate to your
platform.


Linux
-----

To remove mango after it has been installed with a given installation
prefix PREFIX remove the following files and folders (the first three
entries are folders):

  |  *PREFIX/lib/mangopy*
  |  *PREFIX/include/mango* 
  |  *PREFIX/share/mangopy/samples* 
  |  *PREFIX/bin/mango* 
  |  *PREFIX/lib/libmango_core.a*
  |  *PREFIX/lib/libmango_geometry.a*
  |  *PREFIX/lib/libmango_onglut.a*
  |  *PREFIX/lib/libmango_py.a*

The default installation prefix is */usr/local/*.


OSX
---

If you wish to remove a particular version of Mango, delete the folder

  */Library/Frameworks/Mango.framework/Versions/<version>/*

In this case you may wish to make sure symbolic link *Current* that
resides in the same folder does not point to the version removed.

If you wish to remove all versions of Mango, delete the following
folder and symbolic link:

  | */Library/Frameworks/Mango.framework/*
  | */usr/bin/mango*

Admin permissions may be required to remove these files.


Windows
-------

Before removing a particular version of Mango, backup any files not
originally belonging to the installation such as extra scripts or
modules places in the *script* directory:

  *C:\\Program Files\\Mango\\<version>\\script\\*

Next, run the uninstaller for that version which can found in the
"Mango-<version>" entry in the Start menu.

The uninstaller will remove all installation files, the start-menu
entry, and the association with *.py* files. It is designed to leave
files not belonging to the installation alone, so the folder
*C:\\Program Files\\Mango\\<version>* might not be completely removed. It
is safe to manually remove this folder.
