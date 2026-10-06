# Upstream review guide

This branch exists to make the ESP32-S3 experimental motion work easier to review against the real **grblHAL/ESP32 fork lineage**.

## Repository layout

- Upstream parent: `grblHAL/ESP32`
- Fork: `Robik123445/ESP32`
- Review branch: `experimental-motion-s3`
- Full experimental snapshot: `Robik123445/ESP32-Experimental`
- Snapshot source commit: `9bf4cdec7f2124e4f19a0b4fa83acbaf76f36b09`

The original `ESP32-Experimental` repository remains the complete experimental snapshot. This branch is intentionally arranged for review so the ESP32-side changes are visible directly against the fork.

## Experimental motion work

The branch contains the ESP32-side integration for:

- analytic jerk-limited S-curve motion
- 1 kHz Fixed-Time Motion sampling
- ZV input shaping
- optional trajectory smoothing
- motion diagnostics and host-side tests
- ESP32-S3 target support and related driver integration

All experimental motion features are intended to default OFF.

## Changes inside grblHAL submodules

The experimental snapshot also modifies files inside two submodules. To avoid flattening all submodules into hundreds of unrelated files, the modified versions are copied under `review/submodules/` for focused inspection.

### grblHAL/core

Base commit used by the ESP32 fork:

`a61e5e108927bf86e1cf66951ab7cb28895a2e52`

Modified files:

- `planner.c`
- `settings.h`
- `stepper.c`
- `stepper.h`

Review copies:

`review/submodules/core/`

### grblHAL/Plugin_SD_card

Base commit:

`62f1f140c4e3f242e1c4f5b2c29360b17a7ddd8d`

Modified file:

- `fs_littlefs.c`

Review copy:

`review/submodules/sdcard/fs_littlefs.c`

The other submodules in the experimental snapshot match the commits referenced by the ESP32 fork.

## Current hardware-test status

This is still experimental work and physical testing is not yet systematic.

The firmware has been run on a real CNC used for production work. With NEMA23 motors and TB6600-class stepper drivers, the first practical observation is that the motors are noticeably quieter and the motion feels smoother.

At the moment the machine also feels slower than the previous setup. I have not investigated that properly yet, and I have not retuned the machine after enabling the new motion stack. The previous grblHAL configuration was tuned quite aggressively.

My wife also uses the machine as an operator for actual product work. Her first non-technical feedback was simply that the machine feels slower, which I am treating as useful operator feedback rather than a measured performance result.

So, at this stage:

- software integration is advanced
- host-side tests exist
- real hardware has been exercised
- motion feels smoother/quieter in initial use
- speed/acceleration tuning still needs work
- broader physical acceptance testing is still pending

## Suggested review order

1. `docs/MOTION_ARCHITECTURE.md`
2. `docs/EXPERIMENTAL_MOTION.md`
3. `main/experimental/`
4. `main/driver.c` and `main/CMakeLists.txt`
5. `review/submodules/core/`
6. `docs/INPUT_SHAPING.md`
7. `tests/motion/`

This branch is for technical review, not a claim that the motion stack is production-ready.
