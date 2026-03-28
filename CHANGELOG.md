# Changelog

All notable changes to this project are documented in this file.

## [Unreleased]

## [1.0.0] - 2026-03-28

First **stable** release. Supersedes prior **0.8 beta** GitHub pre-releases.

### Added

- **`external_components`** pulling ESPHome `adc` from [PR #7942](https://github.com/esphome/esphome/pull/7942) (`github://pr#7942`) to address ADC deprecation / build issues on newer toolchains.
- Commented **`web_server`** block (port 80, `include_internal`) documenting why it stays off for battery life.
- **BDF fonts** under `fonts/`: `arial_11.bdf`, `arial_16.bdf`, `arial_24.bdf` (bitmap equivalents of the former TTF sizes) for `tiny_font`, `small_font`, `medium_font`.
- **Template `binary_sensor`** `usb_power_present`: treats VBAT above **4.3 V** as USB power (TTGO T-Display behavior). Name corrected to **`"USB Power Present"`** (fixes a stray-quote typo from the private config).
- **`color_yellow`** for the on-screen USB indicator.
- **`secrets.yaml.example`** and README guidance for **one-click** GitHub/Pages builds vs local **`secrets.yaml`** (gitignored).

### Changed

- **GitHub Actions:** bump `actions/checkout` to **v6.0.2**, `actions/upload-artifact` / `actions/download-artifact` to **v7.0.0** / **v8.0.1**, `actions/upload-pages-artifact` to **v4.0.0**, `actions/configure-pages` to **v6.0.0**, and `actions/deploy-pages` to **v5.0.0** (Node.js 20 deprecation on older action pins).
- **Publish workflow:** deploy to GitHub Pages on **push to `main`** (in addition to releases and `workflow_dispatch`) so the live site stays the Actions-built **`static/`** installer + `firmware/`, instead of going stale or being replaced by a branch-based Jekyll build of **README.md**.
- **ESPHome versioning:** **2026.3.1** is the **minimum** verified release; **newer** ESPHome (e.g. current **stable**) is explicitly supported. **Publish** uses **`stable`** so GitHub Pages firmware tracks new releases. **CI** compiles against **`2026.3.1`** and **`stable`** (replacing the old **2024.7.3** pin-only setup).
- **`logger`**: default verbosity set to **`WARN`** (was default / optional `VERBOSE` comment only).
- **Main freezer** Home Assistant entity: **`sensor.main_freezer_temperature`** (was `sensor.atc_d936_temperature`). Forkers with a different freezer sensor should edit this line.
- **Battery `template` sensor**: piecewise voltage-to-percent steps; clamps high voltage to **4.2 V**; guards missing `vcc` state; uses **`last_low_battery_time`** so low-battery handling is not hammered every interval; **does not** run the low-battery deep-sleep script when **`usb_power_present`** is true.
- **Display `lambda`**: row labels at **x = 2**; battery **gauge** (outline, fill by %, terminal nub); **percent text** beside the gauge; **yellow “⚡”** when USB power is detected (when `batterylevel` has state).
- **Wi‑Fi**: **`ssid`** / **`password`** use **`!secret wifi_ssid`** and **`!secret wifi_password`** (ESPHome convention). CI and the Publish workflow run **`cp secrets.yaml.example secrets.yaml`** before compile so strangers and automation need no real credentials.
- **Publish workflow**: firmware build is **inlined** (no longer `esphome/workflows` reusable only) so dummy `secrets.yaml` can be created first.
- **CI workflow**: dummy `secrets` copy step; **`esphome/build-action@v7.1.0`**; removed non-existent **`lilygoT-Display-QuadSensor.factory.yaml`** build step.

### Notes for maintainers

- **Font files** are Arial-derived BDF (see `COPYRIGHT` inside the `.bdf` headers). Confirm redistribution terms match how you use this public repository.
- When PR **#7942** is merged into an ESPHome release you ship, consider **removing** the `external_components` override to avoid pinning an old branch.

### What was intentionally not merged from the private config

- **`api` encryption** and **`ota` password** via `!secret` — kept out of the public “strangers can flash prebuilt firmware” path; use your private `esphome-config` (or local YAML) for hardened API/OTA.
- **Captive fallback AP** name/password from secrets — public config still uses **`Fallback_AP`** without a password in YAML (optional hardening remains in `secrets.yaml.example` for forks that add `!secret` lines).
