<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="docs/images/healthypi6-logo-dark.png">
  <img alt="HealthyPi 6" src="docs/images/healthypi6-logo-light.png" width="380">
</picture>

</div>

# HealthyPi 6 — Hardware

Open hardware design files for HealthyPi 6, a standalone vital-signs monitor with a
modular expansion bus.

The device is built around a tri-core architecture — an STM32H757BI (Cortex-M7 +
Cortex-M4) with an ESP32-C6 for wireless — with a 4" capacitive touchscreen, an isolated
supply, an isolated USB port, and two HealthyLink expansion slots that accept new sensing
front-ends without a board respin.

This repository holds the KiCad schematics and PCB layouts, the HealthyLink interface
definition, the enclosure CAD, and the CE/UKCA declarations of conformity.

## Links

| | |
|---|---|
| Product page | https://www.crowdsupply.com/protocentral/healthypi-6 |
| Firmware (Zephyr) | https://github.com/Protocentral/healthypi-6-fw |
| HealthyPi Studio | https://github.com/Protocentral/healthypi_studio |

## Boards

Design files are published as each board is finalised.

| Board | Revision | Role |
|---|---|---|
| [`healthypi_6_main_v5`](hardware/healthypi_6_main_v5/) | **v5** | STM32H757BI main board — AFEs, power, wireless, both HealthyLink slots |
| [`healthypi_6_display_v7`](hardware/healthypi_6_display_v7/) | **v7** | Display interface board — MIPI-DSI and touch to the panel, backlight driver |
| [`healthylink_compute_v5`](hardware/healthylink_compute_v5/) | **v5** | HealthyLink compute / NPU module — STM32N657, camera, storage |
| [`healthylink_gpio_v2`](hardware/healthylink_gpio_v2/) | **v2** | HealthyLink GPIO breakout — the minimal module example |

## Specifications

Cross-checked against the v5 schematic in this repository.

- **Application processor:** STM32H757BI (LQFP208) — Arm Cortex-M7 @ 400 MHz +
  Cortex-M4 @ 200 MHz
- **Wireless:** ESP32-C6-MINI-1 (Wi-Fi 6 / BLE / 802.15.4), UART and SPI to the host MCU,
  plus its own USB-C for direct flashing
- **Display:** 4" 480×800 capacitive touchscreen — 2-lane MIPI-DSI, touch on I²C,
  TPS61158 backlight boost on the display board
- **Biosignal front-ends:** TI ADS1294R (ECG + thoracic respiration), TI AFE4400 (PPG),
  Bosch BMI323 (motion)
- **Expansion:** 2 × HealthyLink slots, 2 × Hirose DF9-31P-1V per slot (62 pins/slot),
  per-slot NCP380 current-limited load switch with fault reporting
- **Power:** isolated 10 W supply (Traco THM 10-0511); TI BQ24074 linear battery charger,
  Maxim MAX17048 fuel gauge, TPS62A02 system buck
- **Memory and storage:** 32 MB SDRAM (Winbond W9825G6KH), 64 MB QSPI NOR
  (Winbond W25Q512JV), microSD
- **Host interface:** USB-C, USB 2.0 full-speed, galvanically isolated through a
  TI ISOUSB111
- **Main board:** 120 × 70 mm, 6-layer

## HealthyLink expansion bus

HealthyLink is the modular expansion system for HealthyPi 6. Each slot presents 62 pins
across two Hirose DF9-31 board-to-board connectors, carrying two SPI buses, I²C, a UART,
six analog inputs, CAN, per-slot interrupt and reset, and a switched 3V3 module rail. The
main board has two slots (A and B).

Modules are **not hot-swappable** — install them with the device powered off.

The full pin assignment and electrical definition is in
[`docs/HEALTHYLINK.md`](docs/HEALTHYLINK.md). The KiCad symbol for the connector is
[`hardware/healthylink_df9_connector.kicad_sym`](hardware/healthylink_df9_connector.kicad_sym) —
use it as the starting point for your own module.

## Enclosure

[`enclosure/`](enclosure/) holds the CAD for the case, in three forms: the editable
Fusion archive, a neutral STEP export per part, and a print-ready STL per part.

| Part | Size |
|---|---|
| `top` | 125 × 75 × 23 mm |
| `bottom` | 125 × 75 × 14 mm |
| `back-cover-with-slots` | 125 × 75 × 3 mm |
| `back-cover-no-slots` | 125 × 75 × 7.5 mm |
| `battery-holder` | 53 × 35 × 14 mm |
| `push-switch` | 8 × 5 × 4 mm |
| `slide-switch` | 7 × 13 × 5 mm |

Every part appears under the same name in both
[`enclosure/step/`](enclosure/step/) and [`enclosure/stl/`](enclosure/stl/). The Fusion
source is [`enclosure/source/healthypi6-enclosure-v5.f3z`](enclosure/source/); open it if
you want to change the geometry rather than print it as-is. The enclosure is drawn around
the 120 × 70 mm main board, so any board modification that moves a connector or a mounting
hole needs a matching change here.

## Repository layout

| Path | Contents |
|---|---|
| [`hardware/`](hardware/) | One folder per board: KiCad project (schematic + PCB) and exported schematic PDF |
| [`docs/`](docs/) | HealthyLink interface definition and images |
| [`enclosure/`](enclosure/) | Enclosure CAD — Fusion source, STEP and STL per part |
| [`compliance/`](compliance/) | CE and UKCA declarations of conformity, safety insert |

## Working from these files

- For firmware, start at the firmware repository. This repo is the electrical reference:
  pin assignments, AFE wiring, connector pinouts.
- To build a HealthyLink module, read [`docs/HEALTHYLINK.md`](docs/HEALTHYLINK.md) and
  copy [`healthylink_gpio_v2`](hardware/healthylink_gpio_v2/), which is the minimal
  working example.
- To respin a board: both projects are saved in **KiCad 10** format and will not open in
  earlier versions. Footprints are embedded in the board files and symbols are cached in
  the schematics, so the projects open standalone. A handful of 3D models resolve through
  an internal `${PROTOCENTRAL}` path and will show as missing in the 3D viewer; this
  affects rendering only, never the netlist or the layout.

## Important notice

This device is intended for **research, education, and evaluation use only**. It is **not**
a medical diagnostic instrument, and it is not FDA-approved or otherwise cleared for
clinical or consumer diagnostic use. The CE and UKCA declarations in
[`compliance/`](compliance/) cover EMC and electrical safety for the device as an
electronic product — they are not medical device certifications.

Do not use HealthyPi 6 to make medical decisions.

## License

The hardware design files in this repository are licensed under the **CERN Open Hardware
Licence Version 2 – Permissive (CERN-OHL-P v2)**. See [LICENSE](LICENSE).

Firmware and software for HealthyPi 6 are licensed separately under the MIT License in
their own repositories.
