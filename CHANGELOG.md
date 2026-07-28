# Changelog

All notable changes to this project are documented in this file.

## [Unreleased]

## [1.4.3] - 2026-07-28

### Fixed

- **Silent-resume vs OTA race:** honor `resume_silent_checkin` only on soft reboot (`ESP_RST_SW`); do not clear the flag before timer sleep (avoids late-OTA interactive boot). Stale NVS after power-on is ignored and cleared ([#11](https://github.com/shomanjk/ESPhome-Tdisplay-Quad-Sensor/pull/11) / [#12](https://github.com/shomanjk/ESPhome-Tdisplay-Quad-Sensor/pull/12) Codex review).
- **Factory `project.version`:** bump to **1.4.3** in [`lilygoT-Display-QuadSensor.factory.yaml`](lilygoT-Display-QuadSensor.factory.yaml) so local/CI factory builds match the release (Publish already rewrote this at deploy time).

## [1.4.2] - 2026-07-28

### Fixed

- **Silent check-in after queued OTA:** persist `resume_silent_checkin` across soft reboot so a successful OTA during the timer window does not fall into the interactive (backlight-on) boot path and stay awake until a button press ([#7](https://github.com/shomanjk/ESPhome-Tdisplay-Quad-Sensor/pull/7) Codex review).
- **Low-battery sleep re-arm:** clear `last_low_battery_time` when USB power cancels the 3-minute low-battery sleep so a later low reading can arm sleep again ([#6](https://github.com/shomanjk/ESPhome-Tdisplay-Quad-Sensor/pull/6) Codex review).
- **Row labels with `%`:** render `label_*` via `printf("%s:", …)` so custom labels are not treated as format strings ([#7](https://github.com/shomanjk/ESPhome-Tdisplay-Quad-Sensor/pull/7) Codex review).
- **ESP32 advanced opts comment:** document chip revision **≥ 3.1** to match the commented `minimum_chip_revision: "3.1"` example ([#9](https://github.com/shomanjk/ESPhome-Tdisplay-Quad-Sensor/pull/9) Codex review).

### Changed

- Updated device photo (`QuadSensor-Tdisplay.jpg`) on the README and GitHub Pages installer (landed after 1.4.1).

## [1.4.1] - 2026-07-28

### Added

- Commented optional `esp32.framework.advanced` (`minimum_chip_revision`, `sram1_as_iram`) plus README guidance — defaults stay compatible with older T-Display silicon/bootloaders; enable only when boot logs confirm it is safe (prefer a local/adopted overlay).
- **Verified ESPHome:** **2026.7.3** (CI/Publish still track **`stable`**).

## [1.4.0] - 2026-07-28

### Added

- **Factory web image** [`lilygoT-Display-QuadSensor.factory.yaml`](lilygoT-Display-QuadSensor.factory.yaml): `project`, `dashboard_import` (Adopt core YAML from this repo), `improv_serial`, and `name_add_mac_suffix`.
- **Publish / CI** build the factory YAML; Publish injects `project.version` from the workflow version string.

### Changed

- **Device identity:** `tdisplay-quad-sensor` / **T-Display Quad Sensor** (was refrigerator-named).
- **Docs:** flash → Improv Wi‑Fi → Adopt → edit `label_*` / `entity_*`; refrigerator/freezer/room temps called out as an example use case; prebuilt still ships demo entity IDs until customized.
- **Installer copy** (`static/index.md`) matches the adopt flow.

### Removed

- Stale [`project-template-esp32.factory.yaml`](project-template-esp32.factory.yaml) that pointed `dashboard_import` at the upstream ESPHome project-template adopt URL.

## [1.3.0] - 2026-07-28

### Added

- **Silent timer check-in:** on RTC timer wake, keep the backlight off, wait for Wi‑Fi/API, publish battery, linger ~**2 minutes** for ESPHome Device Builder **queued offline OTA**, then re-sleep.
- **`deep_sleep_duration` substitution** (default **`24h`**) shared by component default, button sleep, low-battery sleep, and timer re-sleep so forks can tune the interval in one place.
- **`timer_wake` global** so low-battery auto-sleep is not armed during a short check-in.
- **Generic row substitutions:** `label_1`…`label_4` and `entity_1`…`entity_4` (with matching `sensor_1`…`sensor_4` ids) so forks are not tied to fridge/kitchen naming.

### Changed

- **Deep sleep interval:** **`1h` → `24h`** by default (was waking hourly and staying on with the screen lit until a button press).
- **Boot paths:** button wake / cold boot stay **interactive** (backlight on) until sleep button **release**; only timer wakes use the silent check-in script.
- **Forkability:** display row titles come from **`label_*`**; Home Assistant `entity_id` values from **`entity_*`** (example defaults still use the fridge/kitchen sensors).
- **README:** document interactive vs silent wake, queued OTA window, `deep_sleep_duration`, and generic row substitutions.
## [1.2.0] - 2026-07-25

### Changed

- **Display:** migrate `st7789v` → **`mipi_spi`** / **`model: T-DISPLAY`**; drive backlight on **GPIO4** via `output` + `on_boot` (required on ESPHome 2026.7+).
- **ADC:** remove stale **`github://pr#7942`** `external_components` pin (fix is in current stable).
- **USB detect:** publish from VBatt ADC `on_value` / early boot at **> 4.25 V** (was a polling template at 4.3 V that lagged `update_interval`).
- **Deep sleep:** recoverable wake — **`sleep_duration: 1h`** plus **GPIO35** `wakeup_pin` (`IGNORE`, `allow_other_uses`); button sleeps on **`on_release`** with debounce; ignore first release after EXT wake so the device does not immediately re-sleep.
- **Low battery:** after the 3‑minute delay, re-check USB and only then enter 1h sleep (`script` `mode: restart`).
- **Boot:** early VBatt / USB / battery % refresh so the gauge appears within a few seconds.
- **Forkability:** four Home Assistant `entity_id` values moved to **`substitutions`** at the top of the YAML.
- **Verified ESPHome:** **2026.7.2** (CI/Publish still track **`stable`**).

### Removed

- Brickable deep sleep with **no wake sources** (previous public config could sleep until a hardware RST).

## [1.1.0] - 2026-04-07

### Changed

- **Display (USB bolt):** use **Noto Sans Symbols 2** (`bolt_font`, glyph ⚡) instead of Arial BDF, matching the private `refrigerator-quad-temp-display` config; Arial bitmap fonts do not include U+26A1.
- **Publish workflow:** removed **`release: types: [published]`** trigger; deploy runs only on **push to `main`** and **`workflow_dispatch`**. GitHub Releases are for notes/tags only unless you trigger a manual Publish with a chosen version label.

## [1.0.1] - 2026-03-29

### Changed

- **CI:** single compile job on ESPHome **`stable`** only (removed **2026.3.1** matrix leg).
- **`esphome/build-action`** → **v7.2.0** in CI and Publish (nested Docker actions updated; addresses Node.js 20 deprecation warnings from older `docker/build-push-action` / `docker/setup-buildx-action` pins in **v7.1.0**).
- **README / YAML header:** document **stable** as the supported path; **2026.3.1** called out as **last explicitly verified** ESPHome in this repo.

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
