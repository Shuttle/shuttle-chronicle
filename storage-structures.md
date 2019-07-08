---
title: Storage structures
layout: api
---
# Storage structures

Chronicle makes use of an assortment of binary files to store all data.  A Chronicle data file will have a `.cdf` extension.  These files are organised in various formats to facilitate the applicable use-case.

## Header

Every `.cdf` file will have a header with the following format:

| Position | Name | Type | Description |
| --- | --- | --- | --- |
| 0 | Endianness | byte | 0 = big endian; else little endian. |
| 1..4 | Type Identifier | uint32 | The type should be a unique value that would identify the contents of the file. |
| 5..8 | Version | uint32 | The unique version of the file starting at 1. |
| 9..n | Data | byte[] | The type specific content. |

# Known file types

All the position indicators are relative to the next byte of the last position `n` in the header.

## Master (0)

The master file has a type identifier of `0` and represents the data file configuration:

| Position | Name | Type | Description |
| --- | --- | --- | --- |
| 0..3 | File count | uint | The number of file entries. |

Each file entry has the following format relative to the next byte after the *File count*:

| Position | Name | Type | Description |
| --- | --- | --- | --- |
| 0..1 | Length | ushort | The length of the path. |
| 2..2+Length | Path | byte[] | A string byte array representing the path to the file. |

## Raw Data Stream (1)

The raw data stream has a type identifier of `1` represents sequential data that is stored in the same order that it is received.  

Each entry is saved as follows after the header:

| Position | Name | Type | Description |
| --- | --- | --- | --- |
| 0..7 | Sequence number | ulong | The sequence number of the entry. |
| 8..11 | Length | uint | The length of the data. |
| 12..12+Length | Data | byte[] | An array of bytes that represent the data being stored |

