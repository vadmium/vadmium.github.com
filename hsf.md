---
layout: default
title: High Sierra Format
---

# High Sierra Format #

## HSF ##

[WP:High Sierra Format](https://en.wikipedia.org/wiki/High_Sierra_Format)

## ISO 9660 (CDFS) ##

[WP:ISO 9660](https://en.wikipedia.org/wiki/ISO_9660)

[ECMA-119](http://www.ecma-international.org/publications/standards/Ecma-119.htm)
_Volume and file structure of CDROM for information interchange_

* **sector:**
    Smallest addressable individual part of CD
* **Data Field:**
    User data in a CD sector; e.g. 2048 bytes on a 2352-byte sector
* **Logical Sector:**
    Highest power of two bytes that fits a CD sector data field; minimum 2048&nbsp;B, spanning multiple CD sectors. Numbered from zero.
* **System Area:**
    16 logical sectors (32&nbsp;KiB) reserved at the start of the volume
* **Data Area:**
    Remaining space from logical sector number 16 onwards

---

* **Logical Block:**
    512&nbsp;B or higher power of two; at most the “logical sector” size. Numbered from zero.
* **Extent:**
    Any continuous series of logical blocks
* **File Section:**
    Part of file data recorded in an extent. Recorded in “data area”; there may be more than one file section per file.
* **File Unit:**
    Continuous sequence of logical blocks in an extent (extent within an extent). File units have uniform size and spacing for a given file section. Aligned to the start of a logical sector. Any extended attribute record spans entire first file unit.
    * **size:**
        Logical blocks
* **Interleave Gap:**
    Extra space after each file unit; uniform size for a given file section
* **Extended Attribute Record:**
    May be associated with a file section. If recorded, at beginning of its extent. Spans whole of first file unit in interleaved mode.
    * **length:**
        Logical blocks
* **Data Space:**
    The file units (interleaved mode) or extent (non-interleaved mode) subsequent to any extended attribute record. File data recorded beginning at the start of the data space.
    * **data length:**
        bytes; may be less than available

---

* **byte position:**
    Numbered from one

---

* **Volume Descriptor**
* **File Descriptor**
* **Directory Descriptor**
* **Path Table**

All recorded in “data area”

---

* **volume data area:**
    * volume descriptors
        * root directory
        * path table
            * all directories
* **directory:** must be in a single extent
    * subdirectories
    * file sections
        * [extended attribute record]

## Non-sequential recording (NSR) ##

[ECMA-167](http://www.ecma-international.org/publications/standards/Ecma-167.htm)
_Volume and file structure for write-once and rewritable media using non-sequential recording for information interchange_

### Parts ###

* 1: General
* 2: Volume recognition sequence
* 3: Volume
* 4: File system
* 5: Record structure

### Summary ###

* **byte position:**
    Numbered from zero (different from CDFS)
* **numerical values:**
    Recorded in little endian, two’s compliment

---

* **sector:**
    Data field of smallest addressable independent part of medium
    * **number:**
        Initial 0
* **volume recognition area:**
    Sector-aligned, beginning after first 32&nbsp;KiB of sectors in “volume recognition space”, presumably to correspond with CDFS’s Data Area.
* **volume recognition sequence:**
    Sequence of Volume Structure Descriptors, recorded from the start of the volume recognition area. Terminated by invalid descriptor.
* **Volume Structure Descriptor:**
    Aligned to the start of a sector and padded to the end of its last sector with zeros
    * **Standard Identifier** (byte position 1; 5 bytes):
        Indicates interpretation of Volume Structure Descriptor. Values mentioned: BEA01, BOOT2, CD001, CDW02, NSR02, NSR03, TEA01.
    * **Structure Type** (position 0; 1 byte)
    * **Structure Version** (position 6; 1 byte)
    * **Structure Data** (position 7; 2041 bytes)
* **CD-ROM Volume Descriptor Set:**
    Optional; at start of volume recognition sequence. Details specified by CDFS.
* **Extended Area:**
    All Volume Structure Descriptors except the CD-ROM Volume Descriptor Set must be within an Extended Area. Bracketed by at least one Beginning Extended Area Descriptor and at least one Terminating Extended Area Descriptor.
* **Beginning Extended Area Descriptor:**
    Standard Identifier “BEA01”; Structure Type 0; Structure Version 1; Structure Data zeroed
* **Terminating Extended Area Descriptor:**
    Standard Identifier “TEA01”; Structure Type 0; Structure Version 1; Structure Data zeroed
* **Boot Descriptor:**
    Standard Identifier “BOOT2”

---

* **logical sector:**
    Volume allocation unit. Group of volume sectors. Must be at least 258 logical sectors per volume (there must be a logical sector beyond number 256).
    * **length, size:**
        Equal. Multiple of 512&nbsp;B; able to fit any sector.
    * **number:**
        Consecutive, from 0
* **tag:**
    Descriptor tag. Sixteen-byte structure recorded at start of some descriptors. Factors common CRC and format version handling. Allows damage or corruption recovery: helps identifying structures and context.
    * **Tag Identifier** (byte position 0; 2 bytes):
        Specifies type of descriptor
        * **0:**
            Descriptor format not specified
        * **1–9, 256–266:**
            Specified by ECMA-167
        * **65&#x202F;280–65&#x202F;535:**
            Implementation specified
        * **Other values:**
            Reserved for future standards
    * **Descriptor Version** (position 2; 2 bytes):
        * **2:**
            ECMA-167/2
        * **3:**
            ECMA-167/3
    * **Reserved** (position 5; 1 byte):
        Zero value
    * **Tag Serial Number** (position 6; 2 bytes):
        Identifier for a set of descriptors, or zero. Could be used to identify current descriptors from old descriptors when reusing the space without clearing it.
    * **Descriptor CRC length** (position 10; 2 bytes):
        Number of bytes used for CRC. If zero, the CRC is also zero.
    * **Descriptor CRC** (position 8; 2 bytes):
        CRC of data starting directly after the descriptor tag.
        [CRC-16](https://en.wikipedia.org/wiki/CRC-16) using
        ITU-T [V.41](https://en.wikipedia.org/wiki/V.41) standard polynomial.
        Example: 0x70 0x6A 0x77 → 0x3299.
    * **Tag Location** (position 12; 4 bytes):
        Logical sector number of start of descriptor. Consistency check.
    * **Tag Checksum** (position 4; 1 byte):
        Sum of all other tag bytes (0–3, 5–15)
* **extent_ad:**
    Extent Descriptor; 8 bytes
    * **Extent Length** (byte position 0; 4 bytes):
        In bytes. Less than 2<sup>30</sup>; normally multiple of the logical sector size. Zero length if no extent specified.
    * **Extent Location** (byte position 4; 4 bytes):
        Logical sector number. Zero if no extent is specified.
* **Anchor point:**
    May be at logical sectors 256, _n_ − 256, _n_, and multiples of ⌊_n_/59⌋ other than logical sector 0, where _n_ is the last logical sector number, one less than the total number of logical sectors. Thus there would be at least 59 potential anchor points. The number 59 is chosen to be close to 64 and to avoid the likelihood a single error affecting all anchor points. There must be at least two anchor points that record an Anchor Volume Descriptor Pointer.
* **Anchor Volume Descriptor Pointer:**
    Specifies the first extent of the Main and Reserve Volume Descriptor Sequences
    * **tag** (byte position 0; 16 bytes):
        Tag Identifier 2
    * **Main Volume Descriptor Sequence Extent** (position 16; 8 bytes):
        _Extent_ad_. Extent allocation.
    * **Reserve Volume Descriptor Sequence Extent** (position 24; 8 bytes):
        _Extent_ad_. Extent allocation. Zero length if not specified. Should use separate logical sectors to the Main Volume Descriptor Sequence.
    * **Reserved** (position 32; 480 bytes):
        Zero values
* **Volume Descriptor Sequence:**
    Linked list of extents. Each extent begins with an optional series of volume descriptors, followed by either a Volume Descriptor Pointer, a Terminating Descriptor, or an unrecorded logical sector. There may be trailing logical sectors following this. All Volume Descriptor Sequences specified by Anchor Volume Descriptor Pointers should be equivalent.
* **Primary Volume Descriptor:**
    Identifies a volume
    * **tag** (byte position 0; 16 bytes):
        Tag=1
    * **Volume Descriptor Sequence Number** (position 16; 4 bytes)
    * **Primary Volume Descriptor Number** (position 20; 4 bytes)
    * **Volume Identifier** (position 24; 32 bytes):
        _dstring_
    * **Volume Sequence Number** (position 56; 2 bytes)
    * **Maximum Volume Sequence Number** (position 58; 2 bytes)
    * **Interchange Level** (position 60; 2 bytes)
    * **Maximum Interchange Level** (position 62; 2 bytes)
    * **Character Set List** (position 64; 4 bytes)
    * **Maximum Character Set List** (position 68; 4 bytes)
    * **Volume Set Identifier** (position 72; 128 bytes):
        _dstring_
    * **Descriptor Character Set** (position 200; 64 bytes):
        _charspec_
    * **Explanatory Character Set** (position 264; 64 bytes):
        _charspec_
    * **Volume Abstract** (position 328; 8 bytes):
        _extent_ad_
    * **Volume Copyright Notice** (position 336; 8 bytes):
        _extent_ad_
    * **Application Identifier** (position 344; 32 bytes):
        _regid_
    * **Recording Date and Time** (position 376; 12 bytes):
        _timestamp_
    * **Implementation Identifier** (position 388; 32 bytes):
        _regid_
    * **Implementation Use** (position 420; 64 bytes)
    * **Predecessor Volume Descriptor Sequence Location** (position 484; 4 bytes)
    * **Flags** (position 488; 2 bytes)
    * **Reserved** (position 490; 22 bytes):
        Zeroed

## UDF ##

[WP:Universal
Disk Format](https://en.wikipedia.org/wiki/Universal_Disk_Format)

[OTSA UDF specifications](http://www.osta.org/specs)

* Subset of ISO/IEC 13346 = ECMA-176 = NSR

### 1.02 ###

* logical sector size = physical sector size = logical block size
