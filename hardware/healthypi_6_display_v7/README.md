# HealthyPi 6 Display Board

| | |
|---|---|
| Revision | **v7** |
| Role | Display interface — routes MIPI-DSI and touch I²C from the main board to the 4" panel, and drives the backlight |
| Board | 60 × 33 mm, 2-layer |
| KiCad | 10 |
| Schematic PDF | [`healthypi6-display-v7-schematic.pdf`](healthypi6-display-v7-schematic.pdf) |

## Files

| File | Contents |
|---|---|
| `healthypi_6_display_v7.kicad_pro` / `.kicad_sch` / `.kicad_pcb` | KiCad project for this revision |
| `healthypi6-display-v7-schematic.pdf` | Exported schematic — the human-readable reference |

## Interfaces

- 24-way FFC to the main board: 2-lane MIPI-DSI, touch I²C (`TP_SCL` / `TP_SDA`,
  `TP_INT`, `TP_RST`), panel reset and tearing-effect (`MIPI_RST`, `MIPI_TE`)
- TPS61158 boost driving the panel backlight, enabled by `MIPI_BKL`
- 12-way connector to the display panel
