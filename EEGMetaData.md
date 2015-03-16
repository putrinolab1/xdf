If a stream has the content-type **EEG**, we recommend that meta-data about the stream adheres to the structure and naming laid out in the following. While all meta-data is optional, we recommend that any stream should describe at least the channel labels.

```xml

<channels>            <!-- per-channel meta-data; might be repeated -->
<channel>           <!-- description of one channel, repeated (one for each channel in the time series) -->
<unit>            <!-- measurement unit (strongly preferred unit: microvolts) -->
<type>            <!-- channel content-type (EEG, EMG, EOG, ...) -->
<label>           <!-- channel label, according to labeling scheme;
the preferred labeling scheme for EEG is 10-20 (or the finer-grained 10-5) -->
<location>        <!-- measured location (note: this may be arbitrary but should then include
well-known fiducials (landmarks) for co-registration) -->
<X>             <!-- coordinate axis pointing from the center of the head to the right, in millimeters -->
<Y>             <!-- coordinate axis pointing from the center of the head to the front, in millimeters -->
<Z>             <!-- coordinate axis pointing from the center of the head to the top, in millimeters -->


Unknown end tag for &lt;/location&gt;



<hardware>        <!-- information about hardware properties -->
<model>         <!-- model of the sensor -->
<manufacturer>  <!-- manufacturer of the sensor -->
<coupling>      <!-- type of coupling used (can be Gel, Saline, Dry, Capacitive) -->
<material>      <!-- conductive material of the sensor (e.g. Ag-AgCl, Rubber, Foam, Plastic) -->
<surface>       <!-- type of the contact surface (e.g., Plate, Pins, Bristle, Pad) -->


Unknown end tag for &lt;/hardware&gt;



<impedance>       <!-- measured impedance value during setup, in kOhm -->

<filtering>       <!-- information about the hardware/software filters already applied to the data (e.g. notch) -->
<highpass>      <!-- highpass filter, if any -->
<type>        <!-- type of the filter (FIR, IIR, Analog) -->
<design>      <!-- design of the filter (e.g., Butterworth, Elliptic) -->
<lower>       <!-- lower edge of the transition frequency band (in Hz) -->
<upper>       <!-- upper edge of the transition frequency band (in Hz) -->
<order>       <!-- filter order, if any -->


Unknown end tag for &lt;/highpass&gt;



<lowpass>       <!-- highpass filter, if any -->
<type>        <!-- type of the filter (FIR, IIR, Analog) -->
<design>      <!-- design of the filter (e.g., Butterworth, Elliptic) -->
<lower>       <!-- lower edge of the transition frequency band (in Hz) -->
<upper>       <!-- upper edge of the transition frequency band (in Hz) -->
<order>       <!-- filter order, if applicable -->


Unknown end tag for &lt;/lowpass&gt;



<notch>         <!-- notch filter, if any -->
<type>        <!-- type of the filter (FIR, IIR, Analog) -->
<design>      <!-- design of the filter (e.g., Butterworth, Elliptic) -->
<center>      <!-- center frequency of the notch filter (in Hz) -->
<bandwidth>   <!-- width of the notch frequency band (in Hz), if known -->
<order>       <!-- filter order, if applicable -->


Unknown end tag for &lt;/lowpass&gt;




Unknown end tag for &lt;/filtering&gt;




Unknown end tag for &lt;/channel&gt;




Unknown end tag for &lt;/channels&gt;



<reference>           <!-- signal referencing scheme -->
<label>             <!-- name of the dedicated reference channel(s), if part of the
measured channels (repeated if multiple) -->
<subtracted>        <!-- Yes if a reference signal has already been subtracted from the data, otherwise No -->
<common_average>    <!-- Yes if the subtracted reference signal was a common average, otherwise No -->


Unknown end tag for &lt;/reference&gt;



<fiducials>           <!-- locations of fiducials (in the same space as the signal-carrying channels) -->
<fiducial>          <!-- information about a single fiducial (repeated for each one) -->
<label>           <!-- label of the location (e.g., Nasion, Inion, LPF, RPF); can also cover Ground and Reference -->
<location>        <!-- measured location (same coordinate system as channel locations) -->
<X>             <!-- coordinate axis pointing from the center of the head to the right, in millimeters -->
<Y>             <!-- coordinate axis pointing from the center of the head to the front, in millimeters -->
<Z>             <!-- coordinate axis pointing from the center of the head to the top, in millimeters -->


Unknown end tag for &lt;/location&gt;




Unknown end tag for &lt;/fiducial&gt;




Unknown end tag for &lt;/fiducials&gt;



<cap>                 <!-- EEG cap description -->
<name>              <!-- name of the cap (e.g. EasyCap, ActiCap, CustomBiosemiCapA) -->
<size>              <!-- cap size, usually as head circumference in cm (typical values are 54, 56, or 58) -->
<manufacturer>      <!-- manufacturer of the cap (e.g. BrainProducts) -->
<labelscheme>       <!-- the labeling scheme for the cap (e.g. 10-20, BioSemi-128, OurCustomScheme) -->


Unknown end tag for &lt;/cap&gt;



<amplifier>           <!-- information about the used amplification (e.g. Gain, OpAmps/InAmps...) -->
<settings>          <!-- settings of the amplifier -->


Unknown end tag for &lt;/settings&gt;




Unknown end tag for &lt;/amplifier&gt;



<location_measurement>  <!-- information about the sensor localization system/method -->
<model>               <!-- model name of the system, e.g. CMS10 -->
<manufacturer>        <!-- manufacturer of the measurement system, e.g. Polhemus, Zebris -->
<locationfile>        <!-- file system path of the (backup) location file, if any -->
<settings>            <!-- settings of the location measurement system -->


Unknown end tag for &lt;/settings&gt;




Unknown end tag for </location\_measurement>



<acquisition>
<manufacturer>      <!-- manufacturer of the amplifier (e.g. BioSemi) -->
<model>             <!-- model name of the amplifier (e.g. BrainAmp) -->
<precision>         <!-- the theoretical number of bits precision delivered by the amplifier
(typical values are 8, 16, 24, 32) -->
<compensated_lag>   <!-- amount of hardware/system lag that has been implicitly
compensated for in the stream's time stamps (in seconds) -->


Unknown end tag for &lt;/acquisition&gt;

