[![Build Status](https://github.com/remram44/rrsync/workflows/Test/badge.svg)](https://github.com/remram44/rrsync/actions)
[![Crates.io](https://img.shields.io/crates/v/rrsync.svg)](https://crates.io/crates/rrsync)
[![Documentation](https://docs.rs/rrsync/badge.svg)](https://docs.rs/rrsync)
[![License](https://img.shields.io/crates/l/rrsync.svg)](https://github.com/remram44/rrsync/blob/master/LICENSE.txt)
[![Say Thanks!](https://img.shields.io/badge/Say%20Thanks-!-1EAEDB.svg)](https://saythanks.io/to/remram44)

What is this?
=============

This is an rsync clone written in the [Rust](https://www.rust-lang.org/) programming language. It is intended to provide the functionality of rsync, rdiff, and zsync in one single program, as well as some additions such as caching file signatures to make repeated synchronizations faster. It will also provide a library, allowing to use the functionality in your own programs.

Current status
==============

Core functionality is there. You can index and sync local folders.

The next step is implementing SSH and HTTP remotes.

How to use
==========

Common options: `-X` indicates the location of the index file on the source side, and `-x` the index file on the destination side.

rsync
-----

```
$ rrsync sync some/folder othermachine:folder
```

Pre-computed indices are optional but make the operation faster:

```
$ rrsync index -x folder.idx some/folder
$ ssh othermachine \
  rrsync index -x folder.idx folder
$ rrsync sync -X folder.idx -x othermachine:folder.idx some/folder othermachine:folder
```

rdiff
-----

```
# Same as rdiff (signature/delta/patch)
$ rrsync index -x signature.idx old/folder
$ rrsync diff -o patch.bin -x signature.idx new/folder
$ rrsync patch old/folder patch.bin
```

zsync
-----

```
$ rrsync index -x data.tar.rrsync.idx data.tar
$ rrsync sync -X data.tar.rrsync.idx old/data.tar
# Or over network
$ rrsync sync -X http://example.org/data.tar.rrsync.idx old/data.tar
```

Notes
=====

The rsync algorithm: https://rsync.samba.org/tech_report/
How rsync works: https://rsync.samba.org/how-rsync-works.html

zsync: http://zsync.moria.org.uk/

Compression crate: https://crates.io/crates/flate2
