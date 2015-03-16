# Introduction #

This document contains recommendations for representing meta-data about different kinds of recordings (for example, EEG, NIRS, fMRI, and so on). Please contribute only to those parts that you feel comfortable with, and please feel free to add comments to any other parts that you think need more consideration.

Meta-information is associated with a particular stream, and therefore it can be found in the StreamHeader chunk of a stream. In particular, it is represented as an XML string within the `<desc>` tag.

# Call for contributions #
The most important goal is to lay out and delineate the basic categories of the meta-information thoroughly. Details can be added later, but categories cannot be corrected after the first programs start supporting prematurely standardized content. The second most important topic is to specify details in those areas where you are a domain expert (and thus feel comfortable proposing names), to make sure that different programs can adhere to the same names and conventions when it comes to those details.

# Notes on editing #
When editing please try to be consistent with the current naming/formatting style. The meta-information is presented within `<code language="xml">` and `</code>` tags. Comments are started by the # character. Use two spaces to indent by one level. Make sure to add a space before the end of the line.

Expert note: This is a human-readable description for a simplified XML grammar which does not include attributes.

# Stream content types #
Currently, specifications for meta-data associated with the following content-type values is available:
  * [EEG (for Electroencephalogram)](EEGMetaData.md)
  * [MoCap (for Motion Capture)](MoCapMetaData.md)
  * [Gaze (for gaze / eye tracking parameters)](GazeMetaData.md)
  * [VideoRaw (for uncompressed video)](VideoRawMetaData.md)
  * [VideoCompressed (for compressed video)](VideoCompressedMetaData.md)
  * [Audio](AudioMetaData.md)

Further bits of meta-data that can be associated with a stream are the following:
  * [Human-Subject Information](HumanSubjectMetaData.md)
  * [Recording Environment Information](RecordingEnvironmentMetaData.md)
  * [Experiment Information](ExperimentMetaData.md)
  * [Synchronization Information](Synchronization.md)