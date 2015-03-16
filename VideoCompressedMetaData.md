If a stream has the content-type **VideoCompressed**, we recommend that meta-data about the stream adheres to the structure and naming laid out in the following. While most meta-data is optional, at least the field encoding/codec must be present.

```xml

<encoding>           # specification of how each sample (frame) is encoded
<codec>            # must be present; this is typically the FourCC code of the codec used


Unknown end tag for &lt;/encoding&gt;



# All remaining fields (encoding, display, content, ...): same as in VideoRaw.

```