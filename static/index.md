# About

This project shows four Home Assistant temperature values on the **LilyGO T-Display** (ESP32), plus a battery gauge and USB indicator. It has **not** been tested on T-Display *S3* variants.

<img src="https://github.com/shomanjk/ESPhome-Tdisplay-Quad-Sensor/blob/main/QuadSensor-Tdisplay.jpg?raw=true" alt="Quad Sensor Display screenshot" width="300"/>

# Installation

Use **Chrome** or **Edge** on a desktop (Web Serial is required). Connect the board by USB, then use the button below to flash prebuilt firmware from this site. More detail is in the [GitHub README](https://github.com/shomanjk/ESPhome-Tdisplay-Quad-Sensor#readme).

<script type="module" src="https://unpkg.com/esp-web-tools@10/dist/web/install-button.js?module"></script>

<esp-web-install-button manifest="./firmware/manifest.json">
  <span slot="unsupported">This installer needs <strong>Google Chrome</strong> or <strong>Microsoft Edge</strong> on a computer with Web Serial; it does not work in Safari/Firefox or on iOS.</span>
  <span slot="not-allowed">Open this page over <strong>HTTPS</strong> (GitHub Pages does this automatically).</span>
</esp-web-install-button>
