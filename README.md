# ESPHome Project for using Lilygo T-Display ESP32 device for showing 4 sensor values from Home Assistant on the display

It includes a GitHub workflow that will automatically build the configuration(s) and then deploys a simple 
website via GitHub pages that utilises [ESP Web Tools](https://esphome.github.io/esp-web-tools/) for users to 
easily install your project onto their device.

This YAML is known to work with the original **Lilygo T-diplay**, and has not been tested on any of the T-Display *S3* variants.

**ESPHome:** known good on **2026.3.1** and **newer** (including future stable releases). Older versions than 2026.3.1 are not supported (e.g. BDF fonts and `adc` behavior). GitHub **Pages** builds use **current stable** ESPHome so hosted firmware stays up to date; **CI** checks both **2026.3.1** and **stable**.

<img src="https://github.com/shomanjk/ESPhome-Tdisplay-Quad-Sensor/blob/main/QuadSensor-Tdisplay.jpg?raw=true" alt="Quad Sensor Display screenshot" width="300"/>

## One-click builds and secrets

`lilygoT-Display-QuadSensor.yaml` uses ESPHome’s usual pattern for Wi‑Fi: `ssid` and `password` are `!secret wifi_ssid` and `!secret wifi_password`.

- **Visitors** can still install **prebuilt** firmware from GitHub Pages; they do not need a `secrets.yaml` on their machine.
- **CI and Pages** copy `secrets.yaml.example` to `secrets.yaml` before compile so builds use **dummy** Wi‑Fi strings and never touch your real network.

**Never commit** a real `secrets.yaml`. It is **gitignored**. For **local** `esphome compile` / dashboard, copy `secrets.yaml.example` → `secrets.yaml` and set your real `wifi_ssid` / `wifi_password`. Optional keys in the example (API encryption, OTA, AP password) are only used if you add matching `!secret` lines to your YAML.

## Instructions

1. Visit https://shomanjk.github.io/ESPhome-Tdisplay-Quad-Sensor/ to install this firmware to your Lilygo T-Display
2. If you build locally: copy `secrets.yaml.example` to `secrets.yaml` and set Wi‑Fi (and any other secrets you reference). Edit the YAML to replace the sensor `entity_id` values with those you use in Home Assistant.
   - Make sure to check `Include all branches` so that GitHub Pages is automatically enabled.
  
The button at the bottom right of the display (when USB port is at the bottom) will put it into a *deep sleep mode*, and the button on the right side of the display will wake it back up.

I will update these instructions with more details at a future date.  Please feel free to recommend changes of course.
