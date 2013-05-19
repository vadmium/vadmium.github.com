---
coding: UTF-8
layout: default
title: Rar
---

# Rar #

* Wikipedia: [RAR](https://en.wikipedia.org/wiki/RAR)

* <http://www.forensicswiki.org/wiki/RAR>

  * Rar 4.11 format: <http://www.forensicswiki.org/w/images/5/5b/RARFileStructure.txt>

* Rar format: <https://kthoom.googlecode.com/hg/docs/unrar.html>

## Versions ##

* 2\.0

### 3 ###

RAR 3 features a PPM based compression algorithm developed by Dmitry Shkarin

## Implementations ##

### unRAR (Roshal) ###

Source code by Eugene Roshal

* Licence excludes RAR compression

### Unique RAR Library (<i>unrarlib</i>) ###

<http://www.unrarlib.org>

* By Christian Scheurer; Unix port by Johannes Winkelmann
* Decompression, decryption, version 2.0
* Licence: GPL or except RAR compression
* Source Forge Subversion sources: [unrarlib/trunk/rar2](http://unrarlib.svn.sourceforge.net/viewvc/unrarlib/trunk/rar2)

### <i>unrar</i> (Asselstine) ###

<http://home.gna.org/unrar>

* GPL
* Uses Unique Rar Library
* Gna project: [unrar](https://gna.org/projects/unrar)
* Ben Asselstine
* Code: Gna CVS: [unrar](http://cvs.gna.org/cvsweb/unrar?cvsroot=unrar)
* Wikipedia: [GNA UnRAR](https://en.wikipedia.org/wiki/Unrar#GNA_UnRAR)

### The Unarchiver ###

<http://unarchiver.c3.cx>

* Objective C, OS X oriented
* LGPL
* Google Code project: [theunarchiver](https://code.google.com/p/theunarchiver/)
* Code: Google Code Mercurial XAD Master library: _XADMaster_ directory
  * [Google Code browser](https://code.google.com/p/theunarchiver/source/browse#hg/XADMaster) (100 files per page)
    * [XADRARParser.m](https://code.google.com/p/theunarchiver/source/browse/XADMaster/XADRARParser.m)
  * [Mercurial repository](https://theunarchiver.googlecode.com/hg/XADMaster)
* Wikipedia: [The Unarchiver](https://en.wikipedia.org/wiki/The_Unarchiver)

### VLC ###

Rar access module: <http://git.videolan.org?p=vlc.git;a=tree;f=modules/access/rar>

### Other ###

* Erik Larsson’s RAR archive tools: <http://www.catacombae.org/jlrarx.html>

## Blocks ##

### All blocks ###

Flags:
* 0x8000: LONG_BLOCK
* 0x4000: SKIP_IF_UNKNOWN

### FILE (LHD): 0x74 “t” ###

Flags:
* 0x0004: PASSWORD
* 0x0008: COMMENT
* 0x0010: SOLID
* 0x00E0: DICT [_rarfile_], WINDOW [Unarchiver, Asselstine]
  * 0x00E0: DIRECTORY

### Recovery block (2.00; “protect”): 0x78 “x” ###

* OLD_RECOVERY [_rarfile_], PROTECT [Asselstine]
* Flags set: LONG_BLOCK, SKIP_IF_UNKNOWN

### Sub-block (3.00): 0x7A “z” ###

* SUB [_rarfile_], NEWSUB [Asselstine], SUBBLOCK [VLC]
* Flags set: LONG_BLOCK, SKIP_IF_UNKNOWN

## Recovery records ##

* CRC initialiser for 3.00 sub-block field is 0x0FFF FFFF; apparently a typo [Stack Overflow: [On which data is the File CRC in NEWSUB_HEAD of a RAR Recovery Record based?](http://stackoverflow.com/questions/8126645/on-which-data-is-the-filecrc-in-newsub-head-of-a-rar-recovery-record-based)]
* Protected data is split into 512-byte sectors
* A checksum is stored for each individual protected data sector. The checksum is the least significant 16 bits of the CRC32.
* A specific number of 512-byte recovery record sectors are calculated from the exclusive _or_ of the protected data sectors. They are no necessarily aligned to any disk sector.
