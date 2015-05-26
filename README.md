# Development-tracking Debianization of MyPaint

This is a Debianization of MyPaint which tracks the development master
branch in git. It's where we maintain the official MyPaint project PPAs.

Packaging work done here is intended to be fed into Debian's packaging
workflow in the form of patches to the debian/ tree in their VCS. This
repository has a few extra scripts and READMEs beyond what Debian do,
mostly geared towards PPA creation.

## Install dependencies

Install the packages listed as dependencies in the [control](control)
file, or use the [instructions in the main repository][(depinstrs).

## Local binary builds

These can be made in the normal quick way. Clone MyPaint as normal, then
clone this repository into that MyPaint tree, and build:

    $ cd path/to/your/working/area
    $ git clone https://github.com/mypaint/mypaint.git
    $ cd mypaint
    $ git clone https://github.com/mypaint/debian.git
    $ git submodule update --init --force
    $ fakeroot debian/rules binary

These binary builds should not be released anywhere, but they're handy
for quickly testing that the packages will install and behave.

## PPA builds

This is a work in progress. See `build-ppa-uploads` for the scripting.

The PPA upload builder tailors the master changelog here so that each
generated source tarball and changes has a unique name for each Debian
"series" - a requirement of Ubuntu's PPA autobuilders.  For this to
work, each revision here must look like

    mypaint (1.1.1~alpha-0dev2) UNRELEASED; urgency=low

Where the "0dev" and "UNRELEASED" must be exactly those strings. This
forms the master packaging, so only push entries of this form.

## Versioning

_See [Debian's docs about versioning][debvers], and [Ubuntu's][ubuvers]._

Anything built from this repo must be considered earlier than any real
Debian packages with the same `upstream_version`. To guarantee this:

* Assume Debian's `debian_revision` will always start at `1`.
* Assume Ubuntu's will therefore start at `1ubuntu1`.
* This repo's `debian_revision` will always start with `0devN`

This means that builds stemming from this repo will always be superseded
by any normal version from anywhere else. That's OK, since the
`upstream_version`s encountered here in the `debian/changelog` will
generally be suffixed with `~alpha` or `~beta` (note the tilde).

Practical notes:

* On each new MyPaint "formal version", reset the 0devN string to 0dev1.
* For each modification to the backaging, increment the digits after the
  0dev string (0dev1, 0dev2, 0dev3, ...) and make a new changelog entry.
  The `dch` tool is useful for this.


[depinstrs]: https://github.com/mypaint/mypaint/blob/master/README_LINUX.md#build
[debvers]: https://www.debian.org/doc/debian-policy/ch-controlfields.html#s-f-Version
[ubuvers]: https://help.launchpad.net/Packaging/PPA/BuildingASourcePackage

