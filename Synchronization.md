This information may be included in the XML meta-data of any stream to override defaults on time-series synchronization.

```xml

<synchronization>         # information about synchronization requirements
<offset_mean>          # mean offset (seconds). This value should be subtracted from XDF timestamps before comparing streams. For local LSL generated events, this value is defined to be zero.
<offset_rms>           # root-mean-square offset (seconds). Note that it is very rare for offset distributions to be Gaussian.
<offset_median>        # median offset (seconds).
<offset_5_centile>     # 95% of offsets are greater than this value (seconds)
<offset_95_centile>    # 95% of offsets are less than this value (seconds)
<can_drop_samples>     # whether the stream can have dropped samples (true/false). Typically true for video cameras and video displays and false otherwise.


Unknown end tag for &lt;/synchronization&gt;


```