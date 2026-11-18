# AGENTS.md

## Purpose

A Home Assistant wall/desk display built on a Waveshare ESP32-S3-Touch-LCD-4.3 touchscreen.
It shows the most important HA values (solar/PV status, temperature, humidity) and sleeps until
touched. See [README.md](./README.md) for the full write-up and photos.

## Repository Overview

- Project type: single-file ESPHome firmware (LVGL UI) for an ESP32-S3 board
- Primary technologies: ESPHome (esp-idf framework, min_version 2025.9.0), LVGL, Home Assistant
- Entry point: [esphome-web-esp32-S3-touch-lcd-4.3.yaml](./esphome-web-esp32-S3-touch-lcd-4.3.yaml)
- Important directories: `images/` (documentation screenshots only)

## Setup

No local toolchain. The firmware is built and flashed by the ESPHome integration inside Home Assistant:

1. Add ESPHome to your HA instance and connect the display with the web-based ESPHome flash tool.
2. Open the device in the ESPHome dashboard, paste/adjust the YAML above, and press **Install**.

Hardware prerequisites: an LVGL-capable ESP32-S3 touchscreen flashable with ESPHome
(https://www.waveshare.com/wiki/ESP32-S3-Touch-LCD-4.3), a 3D-printed housing, and optionally
https://www.espboards.dev/tools/esphome-lvgl-designer/ for designing the GUI layout.

Examples of a similar project: https://jtenniswood.github.io/espcontrol/
Guides for ESPHome: https://esphome.io/guides/
Documentation about ESPHome: https://esphome.io/components/
Cookbook with tricks: https://esphome.io/cookbook/

Especially description for prevent burn-in of LCD: https://esphome.io/cookbook/lvgl/#prevent-burn-in-of-lcd
Turn off screen when idle: https://esphome.io/cookbook/lvgl/#turn-off-screen-when-idle

## Build and Run

There is no local build. Compilation and upload (USB cable or over-the-air) happen when you press
**Install** in the ESPHome dashboard.

## Testing

No automated tests. Validate by flashing to the device and checking the display and the ESPHome
logs — the logs show the update rate of each incoming entity, which is the main way to spot
inefficient scripts.

## Code Guidelines

- Keep the whole design in the single YAML file; follow its existing section order
  (`esphome`, `esp32`, `color`, `script`, `lvgl`, sensors, etc.).
- Batch label changes into one `lvgl.label.update:` call — many separate calls hurt performance.
- Use the shared `color:` ids for styling instead of inline hex values.
- Do not commit secrets (Wi-Fi credentials, API keys) into the YAML.

## Agent Instructions

- Read the YAML before editing; the layout is defined by `lvgl` widget ids referenced from `script:` blocks.
- Prefer minimal, focused edits; the file is long and heavily interlinked by widget ids.
- Changes can only be verified on real hardware — say so instead of claiming validation.
- Known open items are marked with `# Todo:` comments in the YAML (LVGL icons, threshold colors).
- Ghost touch events that wake the display are known and intentionally not traced (device is USB-C powered).
