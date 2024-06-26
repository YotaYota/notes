# Debian Package

```
sudo apt install build-essential debhelper debmake devscripts
```

Do not build as root.

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
               python3-setuptools,
               pybuild-plugin-pyproject
Standards-Version: 4.5.0
X-Python3-Version: >= 3.10

Package: python3-foo
Architecture: all
Multi-Arch: foreign
Depends: ${misc:Depends}, ${python3:Depends}
Description: Dummy description
```

**Note**: pybuild-plugin-pyproject is needed when using *pyproject.toml* file.

- `debian/copyright`
- `debian/rules`: a makefile

```
#!/usr/bin/make -f
# You must remove unused comment lines for the released package.
#export DH_VERBOSE = 1
export PYBUILD_NAME = foo
export PYBUILD_INTERPRETERS = python3.10

%:
	dh $@ --with python3 --buildsystem=pybuild
```

- `debian/source/format`: it should contain the version number for the format of the source package, eg
```
3.0 (quilt)
```

### **Step 4**: Build the package

#### debuild

```
$ debuild -us -uc
```

Reiterate until it works. The files will be found in parent directory `ls ..`.

#### pbuilder

```
sudo apt install pbuilder debootstrap devscripts debian-archive-keyring
```

```
sudo pbuilder create --distribution sid --mirror http://ftp.us.debian.org/debian/ --debootstrapopts "--keyring=/usr/share/keyrings/debian-archive-keyring.gpg"
```

```
pdebuild
```

```
sudo pbuilder build ../<source file .dsc>
```

deb file will be under _/var/cache/pbuilder/result/_.

Inspect contents of deb package with

```
dpkg -c <.deb>
```


### **Step 5**: Test the package

```
sudo dpkg -i ../<source package name>_<version>_<architecture>.deb
```


## git-buildpackage

Config file at _debian/gbp.conf_.

- **debian-branch** (default = *master*)
- **upstream-branch** (default = *upstream*)
- **pristine-tar branch** (default = *pristine-tar*)
- **patch-queue branch** (default eg *patch-queue/master*)

[Suggested naming](https://dep-team.pages.debian.net/deps/dep14/)

- **debian-branch**
    - *debian/latest* for the main packaging branch
    - *debian/bookworm* for distribution release
- **upstream-branch**
    - *upstream/latest* for most recent upstream code 

### Steps

### Import upstream package

#### Import dsc

dsc file is metadata file that points at tarballs

```
gbp import-dsc --allow-unauthorized --create-missing-branches <dsc file>
```

**Note**: Omitting `--allow-unauthorized` requires checks against author's gpg keys. 

#### Import orig

```
gbp import-orig -u 0.1 ../package-0.1.tar.gz
```

```
git merge upstream
```

This breaks patches, so do

```
gbp pq rebase
```

resolve conflicts

#### Import ref

1. `git tag upstream/<version>` in upstream-branch
2. `gbp import-ref -u <version>` in debian-branch
3. `gbp dch -N <version>-<debian rev>` and commit debian/changelog
4. `gbp buildpackage`

### Refresh patches

Import patches, then exprt them. This makes diffs cleaner for later, since gbp uses sligthly different sytnax

```
gbp pq import
```
creates patch queue on a branch

```
gbp pq export
```

```
git add -u && git commit -m "refresh patches"
```

Each patch is a single commit

### Pick patch

```
git switch patch-queue/<...>
```

```
git cherry-pick <HASH>
```

```
gbp pq export
```

exports them to debian-branch.

- debian/patches/ will have a new file for the changes
- debian/patches/series will be modified for telling machinery how to apply in order

```
git add debian/ && git commit -m "upstream commit <hash>"
```

### Update changelog

Use `dch` or `gbp dch`

```
dch -v 3.2-4build1+something
```

`gbp dch` generate debian/changelog automatically from previous git commit messages.

- New version `gbp dch -N <new-version>`
- Snapshot `gbp dch -S`
- Release `gbp dch -R`

- fill out changelog
- modify UNRELEASED to our os release version

```
git add debian && git commit -m "finish changelog for 3.2-4build1+something" 
```

### Build package

```
gbp buildpackage -us -uc --git-pristine-tar --git-debian-branch=<branch name>
```

**Note**: `-us -uc` turns off gpg signing

Try in Docker

```
docker run -v $PWD/../dist:/dist:ro --rm --it ubuntu:jammy bash
```

**Note**: Change `$PWD/../dist` to wherever deb packages gets put into

Then do `apt update` and `apt install` on the deb package.

### Build source package (for uploading)

```
gbp buildpackage --git-builder="debuild -S" --git-pristine-tar --git-debian-branch=<branch name>
```

Upload the `_source.changes` file to Launchpad

```
dput ppa:<name of ppa> *_source.changes
```

Launchpad infrastructure builds it fromt there.

## Random

- `quilt` is used for patching.
- `sudo dpkg-buildpackage -r fakeroot -b -uc -us`, `-b` for binary, `-uc` for no crypt sign, `-us` for no source sign.
- `sudo dpkg -i <deb>`
- `sudo apt install -f` install missing dependencies for package
- `/etc/apt/sources.list` uncomment src to be able to do `apt source <pkg>` to install source files
- Go from Unstable (sid) -> Testing -> Stable
- Many 3rd party packages install to /opt/PACKAGE, so that they don't have to think about clashing with other packages. If you're installing into /usr/ you do need to pay some attention to debian policy and other packages, to avoid conflicts.
- generally: /usr/{share,lib,bin} belongs to the package managed system and /usr/local/ belongs to the local sysadmin, not apt. So apt install should *not* go into /usr/local/bin/.

## Source

- [Debian packaging intro](https://wiki.debian.org/Packaging/Intro)
- [Debian packaging learn](https://wiki.debian.org/Packaging/Learn)
- [Pybuild](https://wiki.debian.org/Python/Pybuild)
- [Packaging with Git](https://wiki.debian.org/PackagingWithGit)
- [Building Debian Packages with git-buildpackage](https://honk.sigxcpu.org/projects/git-buildpackage/manual-html/index.html)
