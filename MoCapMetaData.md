If a stream has the content-type **MoCap**, we recommend that meta-data about the stream adheres to the structure and naming laid out in the following. While all meta-data is optional, we recommend that any stream should describe at least for each channel the label, marker and type of the channel.

```xml

<channels>           # specification of the channel layout
<channel>          # information about a single channel (repeated for each)
<label>          # label of the channel
<marker>         # label of the marker that this channel refers to, if any
<object>         # label of the object that this channel refers to, if any
<type>           # type of data in this channel, can be an of the following values:
#  * PositionX, PositionY, PositionZ for euclidean position (strongly preferred unit: meters),
#  * OrientationA, OrientationB, OrientationC, OrientationD for quaternion-based orientations,
#  * OrientationP, OrientationH, OrientationR for Euler angles (pitch/heading/roll)
#  * Confidence for confidence information (preferred unit: normalized)
<unit>           # measurement unit (e.g., meters)


Unknown end tag for &lt;/channel&gt;




Unknown end tag for &lt;/channels&gt;



<acquisition>        # information about the acquisition system
<manufacturer>     # manufacturer of the system
<model>            # model name of the system
<settings>         # settings of the acquisition system


Unknown end tag for &lt;/settings&gt;


<compensated_lag>  # amount of hardware/system lag that has been implicitly
# compensated for in the stream's time stamps (in seconds)


Unknown end tag for &lt;/acquisition&gt;



<setup>              # information about the physical setup (e.g. room layout)
<name>             # name of the setup

<bounds>            # bounding box of the space/room (in the same coordinate system as all others)
<minimum>        # smallest possible position in the operating volume (for each axis)
<X>
<Y>
<Z>


Unknown end tag for &lt;/minimum&gt;


<maximum>        # largest possible position in the operating volume (for each axis)
<X>
<Y>
<Z>


Unknown end tag for &lt;/maximum&gt;




Unknown end tag for &lt;/bounds&gt;



<markers>          # information about point markers in the setup
<marker>         # information about a single marker (repeated for each one)
<label>        # label of the marker (referred to in the <marker> element of <channel>)
# extra information about the marker (e.g., location on the body and the like)


Unknown end tag for &lt;/marker&gt;




Unknown end tag for &lt;/markers&gt;



<objects>          # information about objects (e.g., rigid bodies)
<object>         # information about a single object (repeated for each one)
<label>        # label of the object
<class>        # class of the object (e.g., Rigid for rigid bodies/props, Bone, Skeleton, MarkerSet)


Unknown end tag for &lt;/object&gt;




Unknown end tag for &lt;/objects&gt;



<cameras>              # camera setup
<model>             # camera model
<camera>            # information about a single camera (repeated for each camera)
<label>           # label (or id) of the camera
<position>        # 3-d position of the camera, in meters (arbitrary coordinate system)
<X>
<Y>
<Z>


Unknown end tag for &lt;/position&gt;


<orientation>     # orientation of the camera, specified as a quaternion
<A>
<B>
<C>
<D>


Unknown end tag for &lt;/orientation&gt;


<confidence>      # numeric confidence in the position/orientation (preferably normalized between 0 and 1)
<settings>        # settings of the camera


Unknown end tag for &lt;/settings&gt;




Unknown end tag for &lt;/camera&gt;




Unknown end tag for &lt;/cameras&gt;




Unknown end tag for &lt;/setup&gt;



<filtering>          # filtering applied to the data
<occlusions>       # handling of occlusions (Yes/No)
<swaps>            # handling of marker swaps (Yes/No)
<smoothing>        # Exponential smoothing (Yes/No)
<kalman>           # Kalman filter/smoother (Yes/No)
<rigids>           # system is accounting for rigid-body constraints (Yes/No)
<joints>           # system is accounting for joint constraints in articulated bodies (Yes/No)
<kinematics>       # system is accounting for physical kinematics (Yes/No)
<biomechanics>     # system is accounting for bio-mechanical constraints (Yes/No)


Unknown end tag for &lt;/filtering&gt;


```