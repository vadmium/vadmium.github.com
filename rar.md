---
coding: UTF-8
layout: default
title: Rar
---

# Rar #

* Wikipedia: [Rar file format](https://en.wikipedia.org/wiki/Rar_file_format)
* <http://www.forensicswiki.org/wiki/RAR>
  * Rar 4.11 format: <http://www.forensicswiki.org/w/images/5/5b/RARFileStructure.txt>
* Rar format: <https://codedread.github.io/bitjs/docs/unrar.html>

## Versions ##

* <http://rescene.wikidot.com/rar-versions>
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
* Wikipedia: [The Unarchiver](https://en.wikipedia.org/wiki/The_Unarchiver)

Code: Google Code Mercurial XAD Master library: _XADMaster_ directory

* [Google Code browser](https://code.google.com/p/theunarchiver/source/browse#hg/XADMaster) (100 files per page)
  * [XADRARParser.m](https://code.google.com/p/theunarchiver/source/browse/XADMaster/XADRARParser.m)
* [Mercurial repository](https://theunarchiver.googlecode.com/hg/XADMaster)

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
* 0x0001: SPLIT_BEFORE
* 0x0002: SPLIT_AFTER
* 0x0004: PASSWORD
* 0x0008: COMMENT
* 0x0010: SOLID
* 0x00E0: DICT [_rarfile_], WINDOW [Unarchiver, Asselstine]
  * 0x00E0: DIRECTORY
* 0x1000: EXTTIME

Fields:
* . . .
* 4 bytes: DOS time
  * Bits 25–31: Year − 1980
  * Bits 21­–24: Month (1–12)
  * Bits 16­–20: Day of month (1–31)
  * Bits 11­–15: Hour
  * Bits 5­–10: Minute
  * Bits 0­–4: Second / 2
* . . .
* 4 bytes: File attributes
  * 0x10: Directory
  * 0x20: Archive (?)
  * 0x200: Sparse file (?)
* . . .
* Name string
* If EXTTIME is set, an extended time field:
  * 2 bytes: Flags
    * 0xF000: _mtime_ (adds resolution to the existing DOS time field)
      * 0x8000: VALID
      * 0x4000: If set, adds 1 s to the DOS time
      * 0x3000: Number of bytes encoding the fractional seconds
        * 1: 6553.6 µs units
        * 2: 25.6 µs units
        * 3: 0.1 µs units
    * 0x0F00: _ctime_
    * 0x00F0: _atime_
    * 0x000F: _arctime_
  * 0–3 bytes: Little-endian encoding of fractional _mtime_ seconds
  * 4(?) bytes: If _ctime_ VALID flag set, _ctime_ in DOS time format
  * 0–3 bytes: _ctime_ fractional seconds
  * 4(?) + 0–3 bytes: _atime_
  * 4(?) + 0–3 bytes: _arctime_

### Recovery block (2.00; “protect”): 0x78 “x” ###

* OLD_RECOVERY [_rarfile_], PROTECT [Asselstine]
* Flags set: LONG_BLOCK, SKIP_IF_UNKNOWN

### Sub-block (3.00): 0x7A “z” ###

* SUB [_rarfile_], NEWSUB [Asselstine], SUBBLOCK [VLC]
* Flags set: LONG_BLOCK, SKIP_IF_UNKNOWN

### Volume end block: 0x7B “{” ###

Flags:
* SKIP_IF_UNKNOWN is set
* 0x0001: Archive continues into another volume
* 0x0002: Volume CRC field present
* 0x0004: REVSPACE
* 0x0008: Volume number field present

Fields:
* 4 bytes: CRC-32 of all of the volume data, from the initial marker block
  up to the block before this end block
* 2 bytes: Volume part number, zero based
  (0 => *.part1.rar, 1 => *.part2.rar, etc)
* 7 zero bytes: REVSPACE

## Recovery records ##

* CRC initialiser for 3.00 sub-block field is 0x0FFF FFFF; apparently a typo [Stack Overflow: [On which data is the File CRC in NEWSUB_HEAD of a RAR Recovery Record based?](http://stackoverflow.com/questions/8126645/on-which-data-is-the-filecrc-in-newsub-head-of-a-rar-recovery-record-based)]
* Protected data is split into 512-byte sectors
* A checksum is stored for each individual protected data sector. The checksum is the least significant 16 bits of the CRC32.
* A specific number of 512-byte recovery record sectors are calculated from the exclusive _or_ of the protected data sectors. They are
  not necessarily aligned to any disk sector.
