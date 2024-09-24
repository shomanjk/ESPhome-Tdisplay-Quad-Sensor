# ESPHome Project for using Lilygo T-Display ESP32 device for showing 4 sensor values from Home Assistant on the display

It includes a GitHub workflow that will automatically build the configuration(s) and then deploys a simple 
website via GitHub pages that utilises [ESP Web Tools](https://esphome.github.io/esp-web-tools/) for users to 
easily install your project onto their device.

This YAML is known to work with the original **Lilygo T-diplay**, and has not been tested on any of the T-Display *S3* variants.

<img src="https://github.com/shomanjk/ESPhome-Tdisplay-Quad-Sensor/blob/main/QuadSensor-Tdisplay.jpg?raw=true" alt="Quad Sensor Display screenshot" width="300"/>
 
## Instructions

1. Visit https://shomanjk.github.io/ESPhome-Tdisplay-Quad-Sensor/ to install this firmware to your Lilygo T-Display
2. Edit the YAML to replace the sensor names with those you want to pull from your Home Assistant.
   - Make sure to check `Include all branches` so that GitHub Pages is automatically enabled.
  
The button at the bottom right of the display (when USB port is at the bottom) will put it into a *deep sleep mode*, and the button on the right side of the display will wake it back up.

I will update these instructions with more details at a future date.  Please feel free to recommend changes of course.
