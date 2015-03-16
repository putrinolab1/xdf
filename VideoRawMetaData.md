If a stream has the content-type **VideoRaw**, we recommend that meta-data about the stream adheres to the structure and naming laid out in the following. While most meta-data is optional, at least fields width, height, and color\_format under encoding must be present. Colors are not compressed, i.e., one color channel corresponds to a stream channel (e.g., an RGB color pixel takes up 3 channels).

```xml

<encoding>           # specification of how each sample (frame) is encoded
<width>            # width of each sample, in pixels; must be present
<height>           # height of each sample, in pixels; must be present
<color_channels>   # number of channels per pixel (1 for grayscale, 3 for RGB or YUV, 4 for RGBA)
<color_format>     # the color format; typical values are GRAY (grayscale), RGB (red-greed-blue, in that order), RGBA (RGB with additional alpha channel), or YUV (for Y´CbCr); must be present
<color_space>      # color space that the video is in, e.g. sRGB; assume sRGB for RGB format if missing
<codec>            # should be present, assume RAW if missing


Unknown end tag for &lt;/encoding&gt;





Unknown end tag for &lt;/display&gt;

            # specification of how the images shall be displayed
<origin>           # coordinate origin (can be top-left,bottom-left, or, rarely, top-right or bottom-right); assume top-left if missing
<pixel_aspect>     # aspect ratio (x/y) of the pixels; assume 1.0 if missing
<x_dpi>            # horizontal resolution of the content, in dots per inch
<y_dpi>            # vertical resolution of the content, in dots per inch


Unknown end tag for &lt;/display&gt;





Unknown end tag for &lt;/acquisition&gt;

        # information on how the data was acquired
<manufacturer>     # manufacturer of the acquisition device
<model>            # model of the acquisition device used
<serial_number>    # serial number of the acquisition device
<view>             # view of the camera (e.g., left-eye for a view of the left eye of the subject in an eye tracker)
<distortion>       # distortion parameters of the camera used (using Brown's distortion model)
<cx>             # distortion center in x direction
<cy>             # distortion center in y direction
<fx>             # focal length in pixel units
<fy>             # focal length in pixel units
<p1>             # tangential distortion coefficient 1
<p2>             # tangential distortion coefficient 2
<k1>             # radial distortion coefficient 1
<k2>             # radial distortion coefficient 2
<k3>             # radial distortion coefficient 3


Unknown end tag for &lt;/distortion&gt;




Unknown end tag for &lt;/acquisition&gt;



<content>             # information about the video content
<copyright>        # copyright statement


Unknown end tag for &lt;/content&gt;


```