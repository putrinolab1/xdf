If a stream has the content-type **Audio**, we recommend that meta-data about the stream adheres to the structure and naming laid out in the following.

```xml

<provider>     # information about the audio provider
<api>        # name of the audio API (e.g. DirectX, JACK, WinMM, ...)


Unknown end tag for &lt;/provider&gt;



<channels>     # information about the audio channels
<channel>    # information about a single channel (repeated for each)
<label>    # channel label (e.g. Left, Right, Center, ...)


Unknown end tag for &lt;/channel&gt;




Unknown end tag for &lt;/channels&gt;


```