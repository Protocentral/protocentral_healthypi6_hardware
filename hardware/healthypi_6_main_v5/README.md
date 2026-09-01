# HealthyPi 6 Main Board

| | |
|---|---|
| Revision | **v5** |
| Role | STM32H757BI host, AFEs, power, wireless, 2 × HealthyLink slots |
| Board | 120 × 70 mm, 6-layer |
| KiCad | 10 |
| Schematic PDF | [`healthypi6-main-v5-schematic.pdf`](healthypi6-main-v5-schematic.pdf) |

## Files

| File | Contents |
|---|---|
| `healthypi_6_v5.kicad_pro` / `.kicad_sch` / `.kicad_pcb` | KiCad project for this revision |
| `healthypi6-main-v5-schematic.pdf` | Exported schematic — the human-readable reference |

## Schematic sheets

| Sheet | File | Contents |
|---|---|---|
| MCU | `mcu.kicad_sch` | STM32H757BI, clocks, SWD, buttons, buzzer |
| ECG Frontend | `ecg_afe.kicad_sch` | TI ADS1294R, ECG input protection |
| PPG Frontend | `ppg_sensor.kicad_sch` | TI AFE4400, Bosch BMI323, DE9 patient connector |
| Memory | `memory.kicad_sch` | SDRAM, QSPI NOR, microSD |
| USB | `io.kicad_sch` | USB-C, ISOUSB111 isolator, isolated 10 W supply |
| Power | `power.kicad_sch` | Battery charger, fuel gauge, system rails |
| Power 2 | `power2.kicad_sch` | Per-slot HealthyLink load switches |
| Wireless | `wireless.kicad_sch` | ESP32-C6-MINI-1 and its USB-C |
| connectors | `connectors.kicad_sch` | Display FFC, front-panel and auxiliary connectors |
| healthylink | `healthylink.kicad_sch` | Both HealthyLink slots — see [`docs/HEALTHYLINK.md`](../../docs/HEALTHYLINK.md) |
