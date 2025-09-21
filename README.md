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

Extra Files
-----------

As well as the PKGCONF file describing the package, there are a number
of optional extra files that can be included with a package.

* Patch files - These should be placed in the `patches` directory
within the package, and should be named with a `.patch` extension. They
should be "unified" patch files (created with `diff -u`), and will be
applied to the source code after extraction and before configuration.

* Script files - (not implemented yet). These should be placed in the
`scripts` directory, and referenced in the `PKGCONF` file using the
variables:

  * `PREINSTALL=...`
  * `POSTINSTALL=...`
  * `CHECKINSTALL=...`

They will be included in the final package as the relevant script file
entries.


Compiling a Package
-------------------

Simply run `makepkg` in a package folder and it should compile into
a standard Solaris package file for you.  There are some command line
options:

* -i : Install the package after compiling it.
* -s : Compile and install any dependencies of this package first
       before compiling this package
* -f : Force a clean rebuild of the package from scratch
