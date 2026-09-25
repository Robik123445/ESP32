# ESP32-S3 grblHAL driver source

This directory contains the main driver source for the **Robik123445/ESP32** fork of grblHAL.

The current default configuration targets **ESP32-S3** and enables a **dual-Y / Y2 ganged axis with Y auto-squaring**.

For the current ESP32-S3 feature overview, pin map, build instructions, N16R8-class configuration notes, and clone URL, see the repository root:

**[ESP32-S3 grblHAL README](../README.md)**

Key configuration files:

- `my_machine.h` — board and feature selection
- `boards/generic_s3_map.h` — ESP32-S3 GPIO mapping
- `CMakeLists.txt` — ESP-IDF component build configuration

Current machine configuration includes:

```c
#define BOARD_GENERIC_S3
#define Y_GANGED      1
#define Y_AUTO_SQUARE 1
```

This is an independent development fork of [grblHAL/ESP32](https://github.com/grblHAL/ESP32).
