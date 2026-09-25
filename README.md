# grblHAL for ESP32-S3 — Dual-Y Auto-Squaring CNC Controller

This repository is a fork of [grblHAL/ESP32](https://github.com/grblHAL/ESP32) focused on **ESP32-S3** support for CNC controllers.

If you are looking for **grblHAL on ESP32-S3**, **ESP32-S3 GRBL**, a **dual-Y CNC controller**, or **Y-axis auto-squaring**, this fork contains an ESP32-S3 configuration with a dedicated S3 pin map and a four-motor XYZ + Y2 layout.

> This is an independent development fork, not the official grblHAL ESP32 repository.

## Current ESP32-S3 configuration

The current default branch is configured for:

- **MCU:** ESP32-S3
- **ESP-IDF target:** `esp32s3`
- **Board map:** `BOARD_GENERIC_S3`
- **Axes:** X, Y, Z + secondary Y motor (Y2)
- **Dual Y:** enabled with `Y_GANGED=1`
- **Y auto-squaring:** enabled with `Y_AUTO_SQUARE=1`
- **Flash:** 16 MB in the current `sdkconfig`
- **PSRAM:** octal PSRAM support enabled
- **USB:** TinyUSB CDC support is present in the ESP32-S3 project configuration
- **ESP-IDF:** project dependency lock currently targets 4.4.4

The ESP32-S3-specific configuration can be found in:

- `main/my_machine.h`
- `main/boards/generic_s3_map.h`
- `sdkconfig`
- `dependencies.lock`

## ESP32-S3 pin map

The current `BOARD_GENERIC_S3` mapping uses:

| Function | GPIO |
|---|---:|
| X STEP | 12 |
| X DIR | 9 |
| X LIMIT | 34 |
| Y STEP | 13 |
| Y DIR | 10 |
| Y LIMIT | 35 |
| Z STEP | 14 |
| Z DIR | 11 |
| Z LIMIT | 36 |
| Y2 STEP | 21 |
| Y2 DIR | 22 |
| Y2 LIMIT | 37 |
| Stepper ENABLE | 8 |
| Spindle PWM | 47 |
| Spindle ENABLE | 17 |
| Spindle DIR | 18 |
| Coolant flood | 16 |
| Coolant mist | 15 |
| Reset / E-Stop input | 38 |
| Feed Hold input | 6 |
| Cycle Start input | 7 |
| Probe / AUX input | 2 |
| I2C SDA | 3 |
| I2C SCL | 4 |
| SD MISO | 41 |
| SD MOSI | 42 |
| SD CLK | 40 |
| SD CS | 5 |

**Important:** verify every GPIO against your exact ESP32-S3 module and controller hardware before connecting drivers, switches, spindle/laser hardware, or other peripherals.

## Dual-Y and auto-squaring

The current configuration enables a secondary Y motor:

```c
#define Y_GANGED      1
#define Y_AUTO_SQUARE 1
```

The Generic S3 map assigns the secondary Y motor to:

```text
Y2 STEP  -> GPIO 21
Y2 DIR   -> GPIO 22
Y2 LIMIT -> GPIO 37
```

This allows a dual-motor Y gantry to home both sides independently for mechanical squaring.

## ESP32-S3 / N16R8-class configuration

The current `sdkconfig` targets ESP32-S3 with:

- 16 MB flash
- octal PSRAM support
- 240 MHz CPU configuration
- TinyUSB enabled
- USB CDC enabled in the ESP-IDF configuration

This makes the project suitable for ESP32-S3 modules in the **N16R8 class** when the actual module hardware matches the flash/PSRAM configuration.

## Build with ESP-IDF

Clone this fork, including submodules:

```bash
git clone --recursive https://github.com/Robik123445/ESP32.git
cd ESP32
```

If the repository was cloned without submodules:

```bash
git submodule update --init --recursive
```

Use an ESP-IDF 4.4.x environment. The current dependency lock records **ESP-IDF 4.4.4**.

Set the target and build:

```bash
idf.py set-target esp32s3
idf.py build
```

Flash and open the monitor:

```bash
idf.py -p /dev/ttyUSB0 flash monitor
```

Replace `/dev/ttyUSB0` with the serial port used by your board.

## Configuration

Machine and feature configuration is primarily in:

```text
main/my_machine.h
```

The current fork selects:

```c
#define BOARD_GENERIC_S3
#define Y_GANGED      1
#define Y_AUTO_SQUARE 1
```

The ESP32-S3 pin definitions are in:

```text
main/boards/generic_s3_map.h
```

### Native USB

ESP32-S3 USB/TinyUSB support is included in the project. The active grblHAL serial behavior is also controlled from `main/my_machine.h`.

Check the `USB_SERIAL_CDC` setting before relying on native USB as the primary grblHAL console.

## What this fork changes/focuses on

Compared with a generic ESP32 grblHAL setup, this fork is focused on:

- ESP32-S3 as the MCU target
- ESP32-S3-specific board mapping
- four-motor XYZ + Y2 CNC layouts
- dual-Y ganging
- Y-axis auto-squaring
- ESP32-S3 USB support
- 16 MB flash / octal PSRAM configuration
- spindle PWM, enable and direction outputs
- CNC control inputs and limit-switch mapping

A key ESP32-S3 development commit in this fork is:

[`ESP32-S3 N16R8 support: dual Y autosquaring, pinmap, spindle+laser outputs, USB console`](https://github.com/Robik123445/ESP32/commit/5b668bff482fdb4a243ddbe1f4060cfcb5961b51)

The default branch has continued to evolve since that commit, so use the current files as the source of truth for pin assignments and enabled features.

## Upstream

Original project:

- [grblHAL/ESP32](https://github.com/grblHAL/ESP32)
- [grblHAL](https://github.com/grblHAL)

This fork keeps the upstream grblHAL architecture and licensing while carrying ESP32-S3-oriented configuration and changes.

## License and credits

This project follows the licensing terms of the upstream grblHAL ESP32 driver and its included components. See `COPYING` and the source-file headers for details.

Additional upstream component credits remain with their respective authors, including ESP3D-WEBUI, ESP32 WiFi Manager, Espressif examples, and grblHAL contributors.

---

**Keywords:** grblHAL, GRBL, ESP32-S3, ESP32S3, ESP32-S3 CNC, ESP32-S3 GRBL, CNC controller, dual Y, dual-Y CNC, auto-squaring, Y auto square, ESP-IDF, N16R8, spindle PWM, CNC firmware
