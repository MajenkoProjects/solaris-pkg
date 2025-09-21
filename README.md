Solaris Package System
======================

This is an attempt, using pure `sh`, to create a software
building and packaging system for Solaris 10 (and maybe
other versions) on an UltraSPARC II system (my test system
is an Ultra 60).

It is based loosely around a combination of both Arch's
makepkg system and FreeBSD's ports tree.

It is very much a work in progress and liable to break any time.


How it works
------------

Package compilation is based around a reasonably simple POSIX `sh`
script (NOT a `bash` script, so many things you may be used to just
aren't there!). The script contains two main sections.

1. Declarations and Definitions

These are a list of shell variables that describe the package:

  * VERSION - The version number of the package
  * PACKAGE - The name of the package (must be the same as the
    package directory name)
  * CATEGORY - The category the package is in (must be the same
    as the directory the package directory resides in)
  * SRC - The source filename once downloaded - not needed for GIT downloads
  * URL - The URL to get the source from (see below)
  * DEPENDS - A list of packages that should be compiled and installed
    before this one.

2. Operational Functions

These do the actual work of compiling the sofware. Three are essential:

  * configure() - Configure the source (usually running ./configure etc)
  * build() - Compile the source
  * install() - Install it to `${DESTDIR}`

Compiling a Package
-------------------

Simply run `makepkg` in a package folder and it should compile into
a standard Solaris package file for you.  There are some command line
options:

* -i : Install the package after compiling it.
* -s : Compile and install any dependencies of this package first
       before compiling this package
* -f : Force a clean rebuild of the package from scratch
