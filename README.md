# ESPHome: LilyGO T-Display quad sensor dashboard

**Release 1.2.0** — ESPHome 2026.7 display/ADC migration and recoverable deep sleep (see [CHANGELOG.md](CHANGELOG.md)). When publishing on GitHub, create tag **`v1.2.0`** (see [Releases](https://github.com/shomanjk/ESPhome-Tdisplay-Quad-Sensor/releases)).

Four Home Assistant temperature entities, battery gauge, and a USB-power indicator on the original **LilyGO T-Display** (ESP32). Prebuilt firmware is published via GitHub Pages and [ESP Web Tools](https://esphome.github.io/esp-web-tools/) for browser-based install.

## Install prebuilt firmware (web)

1. Connect the T-Display over USB.
2. Open the installer in **Chrome** or **Edge** ([Web Serial](https://developer.mozilla.org/en-US/docs/Web/API/Web_Serial_API); see [ESP Web Tools](https://esphome.github.io/esp-web-tools/)).

Installer URL (clickable on GitHub; copy-paste if needed):

https://shomanjk.github.io/ESPhome-Tdisplay-Quad-Sensor/

That page is built from [`static/index.md`](static/index.md) by the [Publish workflow](.github/workflows/publish.yml) and includes the flash button plus `firmware/manifest.json` — it is **not** this README file.

### If github.io shows this whole README but no install button

**GitHub Pages** is almost certainly set to **Deploy from a branch** (for example **Build and deployment → Source: Deploy from a branch → `/ (root)`**). That makes Jekyll build the **repository root**, so the homepage becomes **README.md** and you will not get the [ESP Web Tools](https://esphome.github.io/esp-web-tools/) button or prebuilt binaries under `firmware/`.

**Fix:** **Settings → Pages → Build and deployment → Source:** choose **GitHub Actions**. After the next successful [Publish](.github/workflows/publish.yml) run (every push to `main`, or **Actions → Publish → Run workflow**), `https://<user>.github.io/<repo>/` should show the short **Installation** page with the install button.

If you **forked** this repo, use your own Pages URL once Actions publishing works: `https://<your-username>.github.io/<your-repo-name>/`.

## Requirements

- **Hardware:** Original **LilyGO T-Display** (ESP32). This config has **not** been tested on T-Display *S3* or other variants.
- **ESPHome:** **Pages** and **CI** both compile with **current stable**. Treat **2026.7.2** as the **last explicitly verified** release in this repo; use **stable** locally. Older ESPHome (before the `mipi_spi` T-Display model and stock ADC) will not match this YAML.
- **Home Assistant:** The device uses the **native API** (`api:`). Edit the four **`substitutions`** at the top of [`lilygoT-Display-QuadSensor.yaml`](lilygoT-Display-QuadSensor.yaml) (and `unit_of_measurement` if you do not use °F) to match your entities.

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
2. **Local builds:** Copy `secrets.yaml.example` → `secrets.yaml`, set Wi‑Fi, then run `esphome compile lilygoT-Display-QuadSensor.yaml` (or use the ESPHome dashboard with this YAML). Set the four `substitutions` entity IDs to your Home Assistant entities.

### If you fork: enable GitHub Pages

This repo deploys Pages with **GitHub Actions** (see [`.github/workflows/publish.yml`](.github/workflows/publish.yml)), not a `gh-pages` branch. In the fork: **Settings → Pages → Build and deployment → Source:** choose **GitHub Actions**.

The workflow **builds** firmware and **deploys** to Pages on **every push to `main`**, or when you **manually run** the Publish workflow (**Actions → Publish → Run workflow**) on `main`. Creating a GitHub **Release** does not run Publish; merge or push to `main` (or a manual run) updates the live site.

## Buttons and sleep

With the **USB port at the bottom**, release the button at the **bottom right** (**GPIO35**) to enter **deep sleep** for **1 hour** (or until you press the same button again to wake). Low battery (< 30%, and not on USB) waits **3 minutes**, then sleeps for **1 hour** the same way—never with zero wake sources.

Sleep is entered on **button release** (not press) so the wake pin is inactive when EXT0 is armed. After a button wake, the first release is ignored so the device does not immediately re-sleep.

## Technical notes (forks / maintainers)

- **Display:** `mipi_spi` / `model: T-DISPLAY`; backlight on **GPIO4** (`output` + `on_boot`). Do not use deprecated `st7789v` on current ESPHome.
- **Battery / USB:** `adc` on **GPIO34** with a `multiply` filter for the onboard divider; USB present when VBatt **> 4.25 V**. Fonts: `COPYRIGHT` in the `.bdf` files under `fonts/`; the USB bolt uses **Noto Sans Symbols 2** (`fonts/NotoSansSymbols2-Regular.ttf`, OFL in `fonts/OFL-NotoSansSymbols2.txt`).

## Other files

- **[CHANGELOG.md](CHANGELOG.md)** — Release history and maintainer notes.
- **[project-template-esp32.yaml](project-template-esp32.yaml)** — Minimal ESP32 + Wi‑Fi template unrelated to the quad display; useful as a starting point for other devices.

Contributions and issue reports are welcome.
