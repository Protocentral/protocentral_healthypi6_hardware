# HealthyLink Compute Module

| | |
|---|---|
| Revision | **v5** |
| Role | HealthyLink compute / NPU module — STM32N657 with camera and storage |
| Board | 50 × 60 mm, 6-layer |
| Slot | Occupies one HealthyLink slot (J5) |
| KiCad | 10 |
| Schematic PDF | [`healthylink-compute-v5-schematic.pdf`](healthylink-compute-v5-schematic.pdf) |

## Files

| File | Contents |
|---|---|
| `healthylink_compute_v5.kicad_pro` / `.kicad_sch` / `.kicad_pcb` | KiCad project for this revision |
| `healthylink-compute-v5-schematic.pdf` | Exported schematic — the human-readable reference |

## Schematic sheets

Sheet names and filenames do not line up on this project; the table gives both.

| Sheet | File | Contents |
|---|---|---|
| *(root)* | `healthylink_compute_v5.kicad_sch` | HealthyLink connector, AT24CS02 ID EEPROM, write-protect jumper |
| MCU | `mcu.kicad_sch` | STM32N657I0H3Q, MX25UM51245G octal flash, boot configuration, 32.768 kHz |
| MCU2 | `mcu2.kicad_sch` | 48 MHz oscillator, status LEDs, debug header |
| MCU3 | `power.kicad_sch` | STM32N657 power pins and decoupling |
| power | `sys_power.kicad_sch` | TPS2116 power mux, 3 × TPS62A02 bucks, TLV75518 LDO |
| connectivity | `connectivity.kicad_sch` | microSD, USB-C, MIPI-CSI camera header |

## Bus usage

Of the HealthyLink signals, this module uses `SPI4` (`CS_1`), `I2C3`, `USART2`,
`MOD_IRQ_N`, `MOD_RESET_N` and the stacking bus. See
[`docs/HEALTHYLINK.md`](../../docs/HEALTHYLINK.md) for the full pinout.
