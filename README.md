# ESPHome: LilyGO T-Display quad sensor dashboard

Four Home Assistant temperature entities, battery gauge, and a USB-power indicator on the original **LilyGO T-Display** (ESP32). Prebuilt firmware is published via GitHub Pages and [ESP Web Tools](https://esphome.github.io/esp-web-tools/) for browser-based install.

## Install prebuilt firmware (web)

**[Open the installer page](https://shomanjk.github.io/ESPhome-Tdisplay-Quad-Sensor/)** — connect the T-Display over USB, use **Chrome** or **Edge**, and click the install button on that page ([Web Serial](https://developer.mozilla.org/en-US/docs/Web/API/Web_Serial_API) requirement; see [ESP Web Tools](https://esphome.github.io/esp-web-tools/)).

If you **forked** this repo, use your own Pages URL after enabling Actions-based Pages: `https://<your-username>.github.io/<your-repo-name>/` (see below).

## Requirements

- **Hardware:** Original **LilyGO T-Display** (ESP32). This config has **not** been tested on T-Display *S3* or other variants.
- **ESPHome:** **2026.3.1** or newer. Older releases are unsupported (BDF fonts and `adc` behavior differ). GitHub **Pages** builds use **current stable** ESPHome; **CI** compiles against **2026.3.1** and **stable**.
- **Home Assistant:** The device uses the **native API** (`api:`). Edit [`lilygoT-Display-QuadSensor.yaml`](lilygoT-Display-QuadSensor.yaml) and set the four `entity_id` values (and `unit_of_measurement` if you do not use °F) to match your entities.

## What appears on the display

Four labeled rows (**Main Fridge**, **Main Freezer**, **Kitchen**, **Office**) with live values, a **battery** outline with fill and percentage, and a yellow **⚡** when USB power is detected (TTGO T-Display behavior).

<img src="QuadSensor-Tdisplay.jpg" alt="Quad Sensor Display screenshot" width="300"/>

## One-click builds and secrets

[`lilygoT-Display-QuadSensor.yaml`](lilygoT-Display-QuadSensor.yaml) uses `!secret wifi_ssid` and `!secret wifi_password` for Wi‑Fi—those are the **only** secrets referenced by the published YAML.

- **Visitors** can flash **prebuilt** firmware from GitHub Pages without a local `secrets.yaml`.
- **CI and Pages** run `cp secrets.yaml.example secrets.yaml` before compile so automation uses **dummy** Wi‑Fi strings.

**Never commit** a real `secrets.yaml` (it is **gitignored**). For local `esphome compile` or the dashboard, copy [`secrets.yaml.example`](secrets.yaml.example) to `secrets.yaml` and set real Wi‑Fi values. The example also lists optional keys (API encryption, OTA, AP password); they take effect only if **you** add matching `!secret` references in your YAML (the public config intentionally keeps API/OTA open for easy prebuilt flashing).

## Instructions

1. Use the **[installer page](#install-prebuilt-firmware-web)** above (same URL as in that section).
2. **Local builds:** Copy `secrets.yaml.example` → `secrets.yaml`, set Wi‑Fi, then run `esphome compile lilygoT-Display-QuadSensor.yaml` (or use the ESPHome dashboard with this YAML). Replace the sensor `entity_id` values with your Home Assistant entities.

### If you fork: enable GitHub Pages

This repo deploys Pages with **GitHub Actions** (see [`.github/workflows/publish.yml`](.github/workflows/publish.yml)), not a `gh-pages` branch. In the fork: **Settings → Pages → Build and deployment → Source:** choose **GitHub Actions**.

The workflow **builds** firmware on every push to `main` (and on releases). The site is **deployed** to Pages when you **publish a GitHub Release** or **manually run** the Publish workflow (**Actions → Publish → Run workflow**) on `main`.

## Buttons and sleep

With the **USB port at the bottom**, the button at the **bottom right** triggers **deep sleep**; the button on the **right edge** wakes the device. In the YAML, `deep_sleep` wakeup pin configuration is **commented out** because it was reported to prevent sleep from working reliably—adjust GPIOs if your board differs (`GPIO35` is used for the sleep button).

## Technical notes (forks / maintainers)

- **`external_components`:** The config pulls ESPHome’s `adc` from [PR #7942](https://github.com/esphome/esphome/pull/7942) for toolchain compatibility. Remove that block once a release you ship includes the fix. See [CHANGELOG.md](CHANGELOG.md).
- **Battery:** `adc` on **GPIO34** with a `multiply` filter for the onboard divider (see YAML comments). Font redistribution: see `COPYRIGHT` in the `.bdf` files under `fonts/`.

## Other files

- **[CHANGELOG.md](CHANGELOG.md)** — Release history and maintainer notes.
- **[project-template-esp32.yaml](project-template-esp32.yaml)** — Minimal ESP32 + Wi‑Fi template unrelated to the quad display; useful as a starting point for other devices.

Contributions and issue reports are welcome.
