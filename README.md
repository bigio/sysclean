SYSCLEAN(8) - System Manager's Manual

# NAME

**sysclean** - list obsolete files between OpenBSD upgrades

# SYNOPSIS

**sysclean**
\[**-s**&nbsp;|&nbsp;**-f**&nbsp;|&nbsp;**-a**&nbsp;|&nbsp;**-p**]
\[**-i**]

# DESCRIPTION

**sysclean**
is a
perl(1)
script designed to help remove obsolete files between OpenBSD upgrades.

**sysclean**
compares a reference root directory against the currently installed files,
taking files from both the base system and packages into account.

**sysclean**
does not remove any files on the system.
It only reports obsolete filenames or packages using out-of-date libraries.

The options are as follows:

**-s**

> Safe mode.
> **sysclean**
> lists obsolete filenames on the system.
> It excludes any dynamic libraries and all files under the
> */etc*
> directory.
> This is the default mode.

**-f**

> File mode.
> **sysclean**
> will additionally show old libraries that aren't used by any packages, and
> */etc*
> will be inspected.
> It will report base libraries with versions newer than what's expected.

**-a**

> All files mode.
> **sysclean**
> will not exclude filenames used by installed packages from output.

**-p**

> Package mode.
> **sysclean**
> will output package names that are using obsolete files.

**-i**

> With ignored.
> **sysclean**
> will include filenames that are ignored by default, using
> */etc/sysclean.ignore*.

# ENVIRONMENT

`PKG_DBDIR`

> The standard package database directory,
> */var/db/pkg*,
> can be overridden by specifying an alternative directory in the
> `PKG_DBDIR`
> environment variable.

# FILES

*/etc/sysclean.ignore*

> Files to ignore, one per line, with absolute pathnames.
> Shell globbing is supported in pathnames; see
> File::Glob(3p).
> If the pattern matches a directory,
> **sysclean**
> will not inspect it or any files contained within.
> For compatibility with the
> changelist(5)
> file format, the character
> '+'
> is skipped at the beginning of a line.
> Additional files can be included with the
> **@include**
> keyword, for example:

> > @include "/etc/changelist"

# EXAMPLES

Obtain a list of outdated files (without libraries used by packages):

	# sysclean -f
	/usr/lib/libc.so.83.0

Obtain a list of old libraries and the package using them:

	# sysclean -p
	/usr/lib/libc.so.84.1   git-2.7.0
	/usr/lib/libc.so.84.1   gmake-4.1p0

Obtain a list of all outdated files (including used libraries):

	# sysclean -a
	/usr/lib/libc.so.83.0
	/usr/lib/libc.so.84.1

# SEE ALSO

pkg\_info(1),
sysmerge(8)

# HISTORY

The first version of
**sysclean**
was written as
ksh(1)
script in 2015, and rewritten using
perl(1)
in 2016.

# AUTHORS

**sysclean**
was written by
Sebastien Marie &lt;[semarie@online.fr](mailto:semarie@online.fr)&gt;.

OpenBSD 6.1 - March 29, 2017
