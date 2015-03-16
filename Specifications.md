# Introduction #

XDF (extensible data format) is a simple extensible storage format for time series and associated data. An XDF file can contain any number of (multi-channel) streams, each of which comprises an XML-based header that specifies both format and [meta-data](MetaData.md) of the stream, as well as the actual data samples. This document specifies the baseline XDF format, which may be extended in the future; any program claiming XDF support should handle all features in the below baseline specification.

The XDF file format specification is an open file format and is licensed under the GNU GPL. The specifications are subject to discussion with the community; everyone is invited to contribute in the form of comments, suggestions, new ideas, and so on. To this end, everything related to XDF (including the specifications) is available on the web. The specifications are tagged with a version number; extensions, improvements, and changes will be incorporated into a new version of XDF. The first public version of XDF is version 1.0, the version described in this document.


# General comments #

A stream represented in XDF may have a regular or an irregular sampling rate, and each of its multi-channel samples can carry a time stamp in addition to its data values. The format of a stream's data values can be any of int8, int16, int32, int64, float, double, and string, but must be the same for all data values within the stream. The time stamp format is double (in seconds).

The XDF format is (aside from an initial four-byte magic code) a sequence of chunks, and if there is more than one stream, chunks with data from different streams should be interleaved. The chunks associated with a single stream are
  * the **StreamHeader** chunk (containing XML),
  * the **Samples** chunk (containing one or more samples of the stream),
  * the **ClockOffset** chunk (containing an offset for the time stamps of the stream that re-maps each stream’s time stamps into a synchronized time domain),
  * the **StreamFooter** chunk (which indicates that the stream was closed orderly).

Furthermore, an XDF file contains chunks not associated with a particular stream, namely a **FileHeader** chunk at the beginning of the file, and optionally **Boundary** chunks that contain a special 16 byte sequence that help finding chunk boundaries when seeking into the file. Chunks with no associated time information (in particular the FileHeader and the StreamHeader), are located at the beginning of the file in no particular order, except that the FileHeader must always be the first chunk. After that, the Samples chunks follow, approximately sorted by time (that is, the time is approximately monotonically increasing). After all Samples chunks the  StreamFooter chunks follow in no particular order (one for each stream).

All binary fields (such as length fields) are stored as little Endian values. All numeric data types are also little Endian, but big Endian data types could be added as additional types in the future. All string types are assumed to be UTF8 encoded unless otherwise specified.

XDF uses the file extension **.xdf**.


# Conventions used in this document #

The structure of an entity in the file is here formatted as a table of three rows:
  * the first row lists the **names** of sub-entities (enclosed in [.md](.md) brackets),
  * the second row lists **specific values** for these entities (examples or pre-defined values),
  * and the third row lists the **sizes** (in bytes) of these entities if not specified elsewhere.

# Basic structure of an XDF file #

<pre>
[Magic Code] [Chunk] [Chunk] [Chunk] ...<br>
[XDF:] [...] [...] [...] ...<br>
[4] [Variable] [Variable] [Variable] ...<br>
</pre>

An XDF file begins with the 4-byte magic code `XDF:` and is followed by zero or more chunks.

# Chunk #

<pre>
[NumLengthBytes] [Length] [Tag] [Content]<br>
[1, 4, or 8] [...] [Tag number] [Arbitrary]<br>
[1] [As coded in NumLengthBytes] [2] [Variable]<br>
</pre>

The length of each chunk is encoded as a variable-length integer, the size of which is indicated by the first byte (which may be either 1, 4, or 8), followed by either a byte, a 4-byte integer (uint32), or an 8 byte integer (uint64). This is to support a uniform chunk format that can be used to hold both very short chunks and very long chunks without a large overhead. The length refers to the length of the chunk's remainder that follows the `[Length]` item. A program should use the shortest encoding that can hold the chunk data when writing.

The chunk tag defines the type of the chunk. In the following sections, only the `[Content]` item for each of the respective chunk types is discussed. Currently used tags are 1-6, defined in the following:
  1. FileHeader (one per file)
  1. StreamHeader (one per stream)
  1. Samples (zero or more per stream)
  1. ClockOffset (zero or more per stream)
  1. Boundary (zero or more per file)
  1. StreamFooter (one per stream)

Any information that can be represented in any of the existing XML-formatted content areas is preferably stored there instead of in a special chunk type, since XDF has a very scalable process for defining such meta-data. Before a new tag is defined, this tag registry should be queried to avoid collisions. Any newly added chunk must retain the `[NumLengthBytes]`, `[Length]`, and `[Tag]` elements so that programs can read through the chunks without having to parse the content structure.


# FileHeader chunk #

<pre>
[XML UTF8 string]<br>
[Valid XML]<br>
[As determined by chunk length]<br>
</pre>

This chunk must be at the beginning of the file, right after the magic bytes. Right now, only the `<version>` field is mandatory:

```xml

<?xml version="1.0"?>
<info>
<version>1

Unknown end tag for &lt;/version&gt;




Unknown end tag for &lt;/info&gt;


```

The `<version>` element is a floating-point number (e.g., 1.1) and refers to the version of the format specification that is used in the file.

# StreamHeader chunk #

<pre>
[StreamID] [XML UTF8 string]<br>
[Ordinal number] [Valid XML]<br>
[4] [As determined by chunk length]<br>
</pre>

The `[StreamID]` is an arbitrary integer identifier that associates the StreamHeader chunk with the remaining chunks of the same stream. Most recording programs will use ordinal numbers starting from 1.

The XML root element of the header is called `<info>` and must contain at least the elements `<channel_count>`, `<nominal_srate>` and `<channel_format>` to be parsed correctly if the version element is 1.x. Of these, `<channel_count>` is a non-negative integer that encodes the number of channels in the stream, `<channel_format>` is a string from the set {int8, int16, int32, int64, float32, double64, string} with possible later extensions (although recording programs should restrict themselves to these baseline formats wherever possible; of these, int64 has limited platform support as of this writing). The field `<nominal_srate>` contains the nominal sampling rate as declared by the recording device or application. Its value is a floating point number in Hertz. If the stream has an irregular sampling rate (that is, the samples are not spaced evenly in time, for example in an event stream), this value must be 0.

For a minimum level of interpretability, the stream should furthermore include the elements `<name>` (a human-readable identifier of the stream source device or application), `<type>` (the overall content-type of the stream, see [MetaData](http://code.google.com/p/xdf/wiki/MetaData) for currently defined content types), and `<desc>` (the root element for all type-specific meta-data beyond this specification). Official recommendations for laying out certain kinds of domain-specific meta-data are organized at the official XDF specification web page.

The following example shows the minimal XML content as populated by an early client, which in addition includes the fields `<source_id>` (a unique ID or serial number of the data source), `<created_at>` (a creation time stamp of the stream), `<uid>` (a unique ID of the stream), `<session_id>` (for identification of the recording session that this stream belongs to, if multiple), and `<hostname>`, which identifies the machine on which the stream was collected (e.g., possibly facilitating time synchronization).

```xml

<?xml version="1.0"?>
<info>
<name>BioSemi

Unknown end tag for &lt;/name&gt;


<type>EEG

Unknown end tag for &lt;/type&gt;


<channel_count>8

Unknown end tag for </channel\_count>


<nominal_srate>100

Unknown end tag for </nominal\_srate>


<channel_format>float32

Unknown end tag for </channel\_format>


<source_id>453742342

Unknown end tag for </source\_id>


<version>1

Unknown end tag for &lt;/version&gt;


<created_at>203776.201525182

Unknown end tag for </created\_at>


<uid>72fbe5d4-2023-4b65-baec-32580623f75c

Unknown end tag for &lt;/uid&gt;


<session_id>default

Unknown end tag for </session\_id>


<hostname>Jordan

Unknown end tag for &lt;/hostname&gt;


<desc />


Unknown end tag for &lt;/info&gt;


```

# Samples chunk #

<pre>
[StreamID] [NumSamplesBytes] [NumSamples] [Sample 1] ... [Sample N]<br>
[Ordinal number] [1, 4, or 8] [Arbitrary] [As defined by format] ...<br>
[4] [1] [As encoded] [Variable] [Variable] ...<br>
</pre>

The Samples chunk contains a sequence of one or more samples. It belongs to the stream identified by the integer `[StreamID]`. The number of samples in the chunk is a variable-length integer (as previously explained), followed by a sequence of samples. The length of each sample is variable (e.g. when string values are used).

## Structure of Sample ##

<pre>
[TimeStampBytes] [OptionalTimeStamp] [Value 1] [Value 2] ... [Value N]<br>
[0 or 8] [Double, in seconds] [Value as defined by format] ...<br>
[1][8 if TimeStampBytes==8, 0 if TimeStampBytes==0] [Variable] ...<br>
</pre>

A sample in the Samples chunk may or may not have an associated time stamp. Its presence is encoded by the first byte (which may be 0 or 8). The number of Values is determined by the fixed number of channels of the stream that is given in the StreamHeader chunk (in the element `<channel_count>`). The time stamp, if present, is in seconds relative to some epoch.

### Structure of Value ###

For numeric values (these are all little Endian):

<pre>
[double, float, int64, int32, int16 or int8]<br>
[Arbitrary]<br>
[8, 4, 8, 4, 2 or 1]<br>
</pre>

The baseline specification omits unsigned integers to reduce the chance of sign errors, especially if stream meta-data is incomplete or omitted. If bit patterns or other coded data are to be stored, the sign should be treated as a data bit. Ideally, such use is indicated through appropriate choice of the content type or other meta-data.

For non-numeric (i.e., string or binary) values, a variable-length encoding is used:

<pre>
[NumLengthBytes] [Length] [StringContent]<br>
[1, 4, or 8] [...] [Arbitrary]<br>
[1] [As encoded] [Length]<br>
</pre>

# ClockOffset chunk #

<pre>
[StreamID] [CollectionTime] [OffsetValue]<br>
[Ordinal number] [Double in seconds] [Double in seconds]<br>
[4] [8] [8]<br>
</pre>

The ClockOffset chunk contains a single offset measurement that serves to map the time stamps of the given stream into a clock domain that is congruent with other streams. This information is only necessary when the collected data stems from mutually unsynchronized clocks and is collected on a best-effort basis. A high-quality importer may collect all ClockOffset chunks for a given stream and linearly interpolate between them to obtain the time offset for any point in time of the stream. This offset, when added to the stream's own time stamps, will yield time stamps that are in a synchronized domain. A simple incremental processing system might only add the most recent ClockOffset to a given time stamp in the stream, or ignore the offsets altogether (assuming that all streams were collected on the same machine). The best possible alignment is obtained by performing a robust linear fit through the offsets and their respective collection times (collection times are read off the same clock as the stream samples' time stamps).

A recording system producing such offset measurements may collect them every few (e.g., 5 or 10) seconds.

# Boundary chunk #

<pre>
[UUID]<br>
[0x43 0xA5 0x46 0xDC 0xCB 0xF5 0x41 0x0F 0xB3 0x0E 0xD5 0x46 0x73 0x83 0xCB 0xE4]<br>
[16]<br>
</pre>

This UUID can be used when seeking into the file or when repairing a corrupted recording; searching for this signature before the seek point allows to find the beginning of the subsequent chunk. We recommend writing Boundary chunks every 10 seconds.

# StreamFooter chunk #

Each stream must have a StreamHeader and StreamFooter chunk. The StreamFooter chunks are located after all Sample chunks at the very end of the file and indicate that the stream was closed correctly. The content of a StreamFooter chunk is redundant, that is, the information contained therein can always be reconstructed by reading the (entire) XDF file. Its primary use is to facilitate the work of importers, because a StreamFooter chunk can contain pre-computed information, which in general reduces the time needed to import an XDF file.

<pre>
[StreamID] [XML UTF8 string]<br>
[Ordinal number] [Valid XML]<br>
[4] [As determined by chunk length]<br>
</pre>

This example shows some fields that could be present in a StreamFooter chunk. Other examples include an event table for pre-defined events (such as those used in classic BCI experiments) or other metrics derived from the data.

```xml

<?xml version="1.0"?>
<info>
<first_timestamp>10032.5

Unknown end tag for </first\_timestamp>


<last_timestamp>33432.1

Unknown end tag for </last\_timestamp>


<sample_count>345242

Unknown end tag for </sample\_count>


<measured_srate>250.09

Unknown end tag for </measured\_srate>




Unknown end tag for &lt;/info&gt;


```