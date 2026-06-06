# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

ZMK firmware configuration for a BB9900 BLE/USB keyboard with optical trackpad, targeting uConsole (portable Linux device). Runs on nRF52840 via ZitaoTech's ZMK fork.

## Stack

- ZMK firmware (ZitaoTech fork, `bbkeyboard_tp` branch — adds trackpad driver + LED indicator features)
- Zephyr RTOS / Device Tree (`.dts`, `.dtsi`, `.overlay`)
- Kconfig (`.conf`, `Kconfig`, `Kconfig.board`, `Kconfig.defconfig`)
- nRF52840 MCU (ARM Cortex-M4)

## Build

There is no local build toolchain. Firmware compiles via GitHub Actions on every push:

```bash
# Trigger a build: commit and push changes to this repo
git push
# Then check Actions tab for build status and download firmware.zip → bb9900-zmk.uf2
```

The CI workflow (`blank.yml`) delegates to ZMK's `build-user-config.yml@v0.3.0`. The build matrix is defined in `build.yaml`.

## Flashing

Put the keyboard into bootloader mode with `RFN + (\ |)`, then copy `bb9900-zmk.uf2` to the mounted drive.

## Key files

| File | Purpose |
|---|---|
| `config/bb9900.keymap` | Keymap — 3 layers (default, sym, upper). Edit this for key bindings. |
| `config/bb9900.conf` | User-facing config — backlight, LED indicators, trackpad speed, BLE timeouts |
| `config/west.yml` | ZMK dependency — pins ZitaoTech fork at `bbkeyboard_tp` branch |
| `config/boards/bb9900/bb9900.overlay` | Hardware additions over base DTS — I2C trackpad (AVAGO A320 at 0x3B), pinctrl for I2C |
| `config/boards/bb9900/bb9900.dts` | Base board DTS — GPIO matrix, PWM LEDs, battery ADC, flash partitions |
| `config/boards/bb9900/bb9900-pinctrl.dtsi` | Pin control definitions for all peripherals |

## Architecture

The board definition in `config/boards/bb9900/` is the hardware description; `config/bb9900.*` are the user-level config/keymap that overlay it. The `.overlay` file in both locations can extend or override the DTS.

**Key hardware details:**
- Keyboard matrix: 7 rows × 11 cols, `col2row` diode direction
- Trackpad: AVAGO A320 over I2C (`i2c0`, SDA=P0.22, SCL=P1.00, fast mode)
- PWM channels: `pwm0` = layer indicator LED, `pwm1` = keyboard backlight, `pwm2` = caps lock indicator
- BLE profiles: 3 (select with `RFN+1/2/3`); clear with `RFN+ESC`
- USB and BLE are mutually exclusive — USB connection disables BLE

**Keymap layers:**
- `0` (default): standard typing + trackpad mouse
- `1` (SYM, hold Fn): F-keys, navigation, screen controls, bootloader
- `2` (UPPER, hold Alt): BT profile select, system reset

**Custom ZMK features** (only in ZitaoTech fork, not upstream ZMK):
- `CONFIG_INPUT_A320_*` — trackpad driver and polling rate
- `CONFIG_TRACKPAD_SCROLL_INTERVAL`, `CONFIG_TRACKPAD_SPEEDMULTIPLIER_*` — trackpad tuning
- `CONFIG_ZMK_INDICATOR_*` — layer indicator LED with cycle/blink effects
- `CONFIG_ZMK_INDICATOR_CAPSLOCK` — caps lock LED

## Post-implementation checks

<!-- post-implementation-review skill reads this section -->
<!-- No automated checks — validate by pushing and checking GitHub Actions build result -->
