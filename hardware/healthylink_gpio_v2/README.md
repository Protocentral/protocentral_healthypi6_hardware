# HealthyLink GPIO Module

| | |
|---|---|
| Revision | **v2** |
| Role | HealthyLink GPIO breakout — the minimal module example |
| Board | 50 × 62.2 mm, 2-layer |
| Slot | Occupies one HealthyLink slot (J5) |
| KiCad | 10 |
| Schematic PDF | [`healthylink-gpio-v2-schematic.pdf`](healthylink-gpio-v2-schematic.pdf) |

## Files

| File | Contents |
|---|---|
| `healthylink_gpio_v2.kicad_pro` / `.kicad_sch` / `.kicad_pcb` | KiCad project for this revision |
| `healthylink-gpio-v2-schematic.pdf` | Exported schematic — the human-readable reference |

## What it does

A single-sheet board that brings the HealthyLink bus out to two 2×10 headers. It is the
smallest complete module in this repository and the recommended starting point for your
own design: it shows the three things every module needs and nothing else.

- **Connector** — one `HEALTHYLINK_DF9_DUAL` (J5), the two DF9-31 receptacles of one slot
- **Identification** — AT24CS02 EEPROM on `I2C3`, with a write-protect solder jumper
- **Power** — a TPS62A02 buck off the module rail

Both SPI buses, `I2C3`, `USART2`, the six analog inputs, CAN, the stacking bus and the
per-slot interrupt and reset all reach the headers. See
[`docs/HEALTHYLINK.md`](../../docs/HEALTHYLINK.md) for the pinout.
