<!---

This file is used to generate your project datasheet. Please fill in the information below and delete any unused
sections.

You can also include images in this folder and reference them in the markdown. Each image must be less than
512 kb in size, and the combined size of all images must be less than 1 MB.
-->

## How it works

Drives a 640x480 60Hz VGA display via the TinyVGA PMOD, showing a DVD-style bouncing Vensa wordmark logo. A 256x64 1-bit bitmap of the "vensa" text is stored in an on-chip ROM (`bitmap_rom.v`) and rendered at the current logo position. Each frame, the logo position advances one pixel; when an edge of the screen is reached the direction flips and the palette index increments, cycling the logo through 8 colors.

Modules:
- `project.v` — top level: VGA timing, position/bounce FSM, RGB output.
- `hvsync_generator.v` — 640x480 hsync/vsync timing generator.
- `bitmap_rom.v` — 2048-byte ROM holding the 256x64 vensa bitmap.
- `palette.v` — 8-entry 6-bit RRGGBB palette.

## How to test

Connect a TinyVGA PMOD to the output bus and a VGA monitor. Drive the design at 25.175 MHz. After reset (`rst_n` low → high), the vensa wordmark begins bouncing.

Configuration via `ui_in`:
- `ui_in[0]` (`cfg_tile`): when high, the bitmap tiles the entire screen instead of being clipped to its 256x64 bounding box.
- `ui_in[1]` (`cfg_color`): when high, the palette cycles on each bounce; when low, the logo stays white.

## External hardware

- TinyVGA PMOD on the `uo_out` bus (R1/G1/B1/VSync/R0/G0/B0/HSync per `info.yaml`).
- VGA monitor capable of 640x480@60Hz.
