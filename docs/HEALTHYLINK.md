<div align="center">

<picture>
  <source media="(prefers-color-scheme: dark)" srcset="images/healthylink-logo-dark.png">
  <img alt="HealthyLink" src="images/healthylink-logo-light.png" width="300">
</picture>

</div>

# HealthyLink — Interface Definition

HealthyLink is the modular expansion bus on HealthyPi 6. It lets the device gain new
sensing front-ends — EEG, compute, GPIO, your own — without respinning the main board.

Everything below is taken from the **v5 main board** schematic in this repository, which
is the shipping host design.

## Electrical overview

| | |
|---|---|
| Connector | Hirose **DF9-31P-1V(32)**, 2 per slot (Primary + Secondary) |
| Pins per slot | 62 |
| Slots on main board | 2 (Slot A, Slot B) |
| Module rail | Switched 3V3 per slot (`VDD_MOD`) through an NCP380 current-limited load switch |
| Always-on rail | `VDD_MOD_AON`, present whenever the device is powered |
| Rail control | Host-side `LDO_EN_MOD_A` / `LDO_EN_MOD_B`; fault flags `MOD_A_FLT` / `MOD_B_FLT` |
| Per-slot interrupt | `MOD_IRQ_N` — `MOD_IRQ_A` / `MOD_IRQ_B` on the host |
| Per-slot reset | `MOD_RESET_N` — `MOD_RESET_A` / `MOD_RESET_B` on the host |
| Module identification | I²C EEPROM on `I2C3`, read during a power-sequenced bus scan |
| Hot-swap | **Not supported.** Install modules with the device powered off. |

Both slots share `SPI4`, `SPI6`, `I2C3`, `USART2`, the analog inputs, the stacking bus and
CAN. Only the interrupt, reset and rail control are per-slot.

## Pinout

Directions are given **from the host's point of view**. Each slot is two DF9-31
connectors: pins 1–31 on the Primary, pins 32–62 on the Secondary.

### Primary connector (pins 1–31)

| Pin | Signal | Direction | Notes |
|---:|---|---|---|
| 1 | `VDD_MOD` | Power | Switched 3V3 module rail |
| 2 | `VDD_MOD` | Power | |
| 3 | `VDD_MOD` | Power | |
| 4 | `VDD_MOD` | Power | |
| 5 | `SPI4_SCK` | Host → Module | Primary SPI bus |
| 6 | `GND` | Power | |
| 7 | `SPI4_MOSI` | Host → Module | |
| 8 | `GND` | Power | |
| 9 | `SPI4_MISO` | Module → Host | |
| 10 | `GND` | Power | |
| 11 | `SPI4_CS_1` | Host → Module | Chip select 1 |
| 12 | `RSVD` | — | Reserved, do not connect |
| 13 | `SPI4_CS_2` | Host → Module | Chip select 2 |
| 14 | `RSVD` | — | Reserved, do not connect |
| 15 | `I2C3_SCL` | Bidirectional | Module ID EEPROM and module I²C devices |
| 16 | `RSVD` | — | Reserved, do not connect |
| 17 | `I2C3_SDA` | Bidirectional | |
| 18 | `RSVD` | — | Reserved, do not connect |
| 19 | `USART2_TX` | Host → Module | |
| 20 | `AGND` | Power | Analog ground |
| 21 | `USART2_RX` | Module → Host | |
| 22 | `AGND` | Power | |
| 23 | `USART2_RTS` | Host → Module | |
| 24 | `AGND` | Power | |
| 25 | `USART2_CTS` | Module → Host | |
| 26 | `AGND` | Power | |
| 27 | `MOD_IRQ_N` | Module → Host | Per-slot interrupt, active low |
| 28 | `VDD_MOD_AON` | Power | Always-on module rail |
| 29 | `MOD_RESET_N` | Host → Module | Per-slot reset, active low |
| 30 | `GND` | Power | |
| 31 | `GND` | Power | |

### Secondary connector (pins 32–62)

| Pin | Signal | Direction | Notes |
|---:|---|---|---|
| 32 | `VDD_MOD` | Power | |
| 33 | `GND` | Power | |
| 34 | `VDD_MOD` | Power | |
| 35 | `GND` | Power | |
| 36 | `SPI6_SCK` | Host → Module | Secondary SPI bus |
| 37 | `GND` | Power | |
| 38 | `SPI6_MOSI` | Host → Module | |
| 39 | `GND` | Power | |
| 40 | `SPI6_MISO` | Module → Host | |
| 41 | `GND` | Power | |
| 42 | `SPI6_CS_1` | Host → Module | Chip select 1 |
| 43 | `RSVD` | — | Reserved, do not connect |
| 44 | `SPI6_CS_2` | Host → Module | Chip select 2 |
| 45 | `STACK_SCK` | Module ↔ Module | Stacking bus, not driven by the host |
| 46 | `HL_ADC_CH0` | Module → Host | Analog input |
| 47 | `STACK_MOSI` | Module ↔ Module | |
| 48 | `HL_ADC_CH1` | Module → Host | |
| 49 | `STACK_MISO` | Module ↔ Module | |
| 50 | `HL_ADC_CH2` | Module → Host | |
| 51 | `STACK_CS` | Module ↔ Module | |
| 52 | `HL_ADC_CH3` | Module → Host | |
| 53 | `MOD_ADDR` | Module → Host | Module address strap |
| 54 | `HL_ADC_CH4` | Module → Host | |
| 55 | `AUX_GPIO0` | Bidirectional | General-purpose |
| 56 | `HL_ADC_CH5` | Module → Host | |
| 57 | `AUX_GPIO1` | Bidirectional | |
| 58 | `FDCAN1_TX` | Host → Module | |
| 59 | `AUX_GPIO2` | Bidirectional | |
| 60 | `FDCAN1_RX` | Module → Host | |
| 61 | `NC` | — | Not connected |
| 62 | `GND` | Power | |

The connector shells carry mounting pins (`MP`) which are tied to ground on the main board.

## Power sequencing

A slot's `VDD_MOD` rail comes up only when the host asserts that slot's enable
(`LDO_EN_MOD_A` / `LDO_EN_MOD_B`). The load switch reports over-current back to the host on
`MOD_A_FLT` / `MOD_B_FLT`, so a shorted module is detected and the slot stays down.

`VDD_MOD_AON` is not switched. Draw only what a module needs to stay identifiable while
its main rail is off.

## Module identification

Modules are identified by an **EEPROM on I²C3**, not by strapping GPIOs. The host powers a
slot, scans the bus, reads the module's identity from its EEPROM, and loads the matching
driver. Earlier designs used dedicated `MOD_ID` pins; those were freed, and the EEPROM is
now the identification path.

## Reference modules

| Module | What it adds |
|---|---|
| [`healthylink_gpio_v2`](../hardware/healthylink_gpio_v2/) | Plain GPIO breakout — the minimal example |
| [`healthylink_compute_v5`](../hardware/healthylink_compute_v5/) | STM32N657 compute / NPU module with camera and storage |

## Building your own module

1. Start from [`healthylink_gpio_v2`](../hardware/healthylink_gpio_v2/) — the smallest
   complete module in this repo — or use the connector symbol at
   [`hardware/healthylink_df9_connector.kicad_sym`](../hardware/healthylink_df9_connector.kicad_sym),
   which carries a host and a module variant, both matching the pinout above.
2. Fit an I²C EEPROM and program it with your module's identity, or the host will not
   enumerate your board.
3. Respect the switched rail: your module must tolerate `VDD_MOD` being held off at boot,
   and must not back-feed the bus from `VDD_MOD_AON`.
4. Stay inside the slot's current limit — the load switch will trip and drop the slot.
5. Leave `RSVD` and `NC` pins unconnected.
