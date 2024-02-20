# Debian Package

```
sudo apt install build-essential debhelper debmake devscripts
```

---

A Debian package is a collection of files that allow for applications or libraries to be distributed via the package management system.

The aim of packaging is to allow the automation of installing, upgrading, configuring, and removing computer programs for Debian in a consistent manner.

A package consists of one source package, and one or more binary packages.

- Binary packages contain executables, standard configuration files, other resources required for executables to run.
- Source packages contain the upstream source distribution, configuration for the package build system, list of runtime dependencies and conflicting packages, a machine-readable description of copyright and license information, initial configuration for the software, and more.

The Debian Policy specifies the standard format for a package, which all packages must follow. 

The source package (.dsc) and binary packages (.deb) will be built for you by tools such as dpkg-buildpackage.

---

- upstream tarball: tar.gz containing software written by upstream developer.
- source package: built from upstream source.
- binary package: built from source package. Is distributed and installed.

## Source Package

The simplest source package consists of three files:

- The **upstream tarball**, renamed according to the naming convention
- A **debian** directory containing the changes made to upstream source, plus all the files required for the creation of a binary package.
- A **description file** (with .dsc extension), which contains metadata for the above two files. 

## The packaging workflow (Manual)

### **Step 1**: Rename the upstream tarball

Naming convention: `<source package name>_<upstream version number>.orig.tar.gz`.
The source package name should be all lower case, and can contain letters, digits, and dashes.

```
mv foo-1.0.tar.gz foo_1.0.orig.tar.gz
```

### **Step 2**: Unpack the upstream tarball

The source should unpack into a directory of the same name and upstream version with a hyphen in between (not an underscore),
so the upstream tarball should unpack into a directory called `<source package name>-<upstream version number>`

```
foo-1.0
```

### **Step 3**: Add the debian.tar.gz files

**Note**: Use
```
debmake
```
to create sensible template files. They need to be modified.

---

`cd` into extracted tarball and create `debian/` directory, then add the following files

- `debian/changelog` This is the log of changes to the Debian package. It has a standard format, use the `dch` tool.
**Note**: version can be eg `1.9.2-4`.

```
dch --create -v <upstream version>-<Debian version> --package <source package name>
```

- `debian/control`: describes the source and binary package, and gives some information about them.
Eg

```
Source: foo
Section: python
Priority: optional
Maintainer: noone <noone@nocomp.com>
Build-Depends: debhelper-compat (= 12),
               dh-python,
               python3-all,
               python3-setuptools
Standards-Version: 4.5.0
X-Python3-Version: >= 3.10

Package: foo
Architecture: all
Multi-Arch: foreign
Depends: ${misc:Depends}, ${python3:Depends}
Description: Dummy description
```

- `debian/copyright`
- `debian/rules`: a makefile

```
#!/usr/bin/make -f
# You must remove unused comment lines for the released package.
#export DH_VERBOSE = 1
export PYBUILD_NAME = foo

%:
	dh $@ --with python3 --buildsystem=pybuild
```

- `debian/source/format`: it should contain the version number for the format of the source package, eg
```
3.0 (quilt)
```

### **Step 4**: Build the package

```
$ debuild -us -uc
```

Reiterate until it works. The files will be found in parent directory `ls ..`.

### **Step 5**: Test the package

```
sudo dpkg -i ../<source package name>_<version>_<architecture>.deb
```

## Source

- [Debian packaging intro](https://wiki.debian.org/Packaging/Intro)
