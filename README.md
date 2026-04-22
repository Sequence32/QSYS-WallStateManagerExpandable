# Wall State Manager v2

**Q-SYS Plugin** — Expandable sensor override interface for physical combinable space walls with configurable wall count.

| Field | Value |
|---|---|
| **Version** | 2.0.1 |
| **Author** | Dustin Bennett — TEL Systems |
| **Plugin ID** | `dstn.qsys.wall-state-manager-v2` |
| **Platform** | Q-SYS Designer |

---

## Overview

Wall State Manager v2 provides a centralized interface for monitoring and controlling combinable room divider walls via physical sensor inputs. The wall count is fully configurable through the plugin's **Properties** tab — no code changes required.

### Key Features

- **Configurable wall count** — Set 1 to 50 walls directly in the Properties tab; controls, layout, and pins are generated dynamically
- **Physical sensor integration** — Accepts GPIO/digital inputs representing physical wall sensor states
- **Inverted sensor logic** — Sensor HIGH = wall closed, Sensor LOW = wall open (standard contact closure behavior)
- **Manual bypass mode** — Override sensor readings with manual toggle controls when needed
- **Automatic re-sync** — Disabling bypass immediately re-syncs all wall states to their physical sensor readings
- **Per-wall output pins** — Each wall state is output individually for downstream plugin consumption

---

## Installation

1. Copy `Wall_State_Manager_v2.qplug` into your Q-SYS plugin directory:
   ```
   Documents\QSC\Q-Sys Designer\Plugins\
   ```
2. Restart Q-SYS Designer or refresh the plugin list.
3. Drag **Wall State Manager v2** from the plugin list into your design.

---

## Configuration

### Properties

| Property | Type | Range | Default | Description |
|---|---|---|---|---|
| **Number of Walls** | Integer | 1–50 | 2 | Number of wall zones to manage. Controls, pins, and layout update automatically. |

Set this value in the **Properties** panel after placing the plugin in your design. The plugin dynamically generates all controls, I/O pins, and the UI layout based on this value.

---

## Controls & Pin Mapping

For each wall `i` (where `i` = 1 to the configured wall count):

| Control | Type | Pin Style | Description |
|---|---|---|---|
| `Sensor_i_In` | LED Indicator | Input | Physical sensor reading for wall `i` |
| `Wall_i_State` | Toggle Button | Both | Current evaluated state of wall `i` (manual or sensor-driven) |
| `Wall_i_Out` | LED Indicator | Output | Final wall state output for downstream consumption |
| `Bypass_Enable` | Toggle Button | Both | Global manual override toggle |

### Pin Wiring

- **`Sensor_i_In`** — Connect to GPIO or Core digital input representing the physical wall sensor
- **`Wall_i_Out`** — Route to downstream plugins (e.g., Room Combiner, PTZ Camera Controller, Touch Panel UI)

---

## Operating Modes

### Normal Mode (Bypass OFF)

- Wall state toggles are **disabled** (read-only)
- Wall states automatically track their corresponding physical sensor inputs
- Sensor changes are reflected immediately on both the state toggle and the output pin
- **Inverted logic**: Sensor HIGH → Wall Closed (`false`), Sensor LOW → Wall Open (`true`)

### Bypass Mode (Bypass ON)

- Wall state toggles become **enabled** for manual control
- Sensor input changes are ignored while bypass is active
- Each wall can be independently toggled open or closed
- Output pins reflect the manual toggle state

### Transition Behavior

- **Bypass ON → OFF**: All wall states immediately re-sync to their physical sensor readings
- **Bypass OFF → ON**: Wall states freeze at their current sensor-driven values; manual override becomes available

---

## UI Layout

The plugin renders a compact control panel organized in a 4-column grid:

1. **Header** — Displays plugin name and configured wall count
2. **Bypass Enable** — Full-width red toggle button for manual override
3. **Wall States** — Toggle buttons for each wall (4 per row)
4. **Sensor Inputs** — Green LED indicators showing raw sensor readings
5. **System Outputs** — Blue LED indicators showing final evaluated outputs

---

## Changelog

### v2.0.1
- Increased maximum wall count from 10 to **50**
- Version bump

### v2.0.0
- Initial v2 release
- Fully dynamic wall count via Properties (replaces hardcoded wall definitions)
- Dynamic control generation, pin mapping, and UI layout
- Bypass/manual override system with automatic re-sync

---

## License

Proprietary — TEL Systems. Internal use only.
