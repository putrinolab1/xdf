If a stream has the content-type **Gaze**, we recommend that meta-data about the stream adheres to the structure and naming laid out in the following. While all meta-data is optional, we recommend that any stream should describe at least for each channel the label, eye, type and unit parameters.

```xml

<channels>           # specification of the channel layout
<channel>          # information about a single channel (repeated for each)
<label>          # label of the channel
<eye>            # which eye the channel is referring to (can be left, right, or both)
<type>           # type of data in this channel, can be an of the following values:
#  * ScreenX, ScreenY: screen coordinates of the gaze cursor (can also refer to a scene image), usually in pixels,
#  * DirectionX,DirectionY,DirectionZ: 3d gaze vector in some coordinate system
#  * PositionX,PositionY,PositionZ: 3d position of the eye center in some coordinate system
#  * IntersectionX, IntersectionY, IntersectionZ: 2d or 3d position of the intersection point with a plane (in some coordinate system)
#  * HeadX, HeadY, HeadZ: 3d location of the head center in some coordinate system,
#  * PupilX, PupilY, PupilZ: 2d or 3d location of the pupil center in some coordinate system,
#  * ReflexX, ReflexY, ReflexZ: 2d or 3d location of the illuminator's reflection point in some coordinate system,
#  * Radius or Diameter: the overall pupil radius or diameter (usually in mm or pixels),
#  * RadiusX,RadiusY: horizontal and vertical pupil radius
#  * DiameterX,DiameterY: horizontal and vertical pupil diameter
#  * Confidence for confidence information (preferred unit: normalized)
#  * FrameNumber: frame number that the parameters were calculated from
#  * PlaneNumber or ObjectId: number or identifier of the object that was intersected by the gaze vector
<unit>           # measurement unit (e.g., pixels, mm, normalized)
<coordinate_system> # coordinate system of the respective parameter, can be world-space, object-space, camera-space, or image-space


Unknown end tag for &lt;/channel&gt;




Unknown end tag for &lt;/channels&gt;



<acquisition>        # information about the acquisition system
<manufacturer>     # manufacturer of the device
<model>            # model name of the device
<serial_number>    # serial number of the device


Unknown end tag for &lt;/acquisition&gt;


```