# Wall State Manager v2

An expandable Q-SYS Plugin providing a sensor override interface for physical combinable space walls with **configurable wall count**.

## What's New vs v1

| Feature | v1 | v2 |
|---------|----|----|
| Wall count | Fixed at 2 | Configurable 1–10 via Properties |
| Controls | Hardcoded `Wall_1`, `Wall_2` | Dynamically generated `Wall_1` through `Wall_N` |
| Layout | Static 2-column | Auto-arranged grid (4 per row) |
| Core logic | Identical | Identical (inverted sensor logic, bypass mode) |

> **v1 is preserved untouched** in the [WallStateManager](https://github.com/Sequence32/WallStateManager) repo.

## Overview

This plugin acts as a gateway between physical wall sensors (GPIO/digital inputs) and the rest of the Q-SYS control system. It evaluates sensor states and provides clean boolean outputs that downstream plugins (UI controllers, camera controllers, video routers) can consume. A manual bypass mode allows operators to override sensor readings for testing or maintenance.

## Features

- **Configurable Wall Count** — Set 1 to 10 walls via the Properties panel
- **Dynamic Control Generation** — Sensor inputs, state toggles, and outputs are created automatically
- **Inverted Sensor Logic** — Sensor HIGH = wall closed (output LOW), Sensor LOW = wall open (output HIGH)
- **Manual Override (Bypass)** — Global toggle enables direct control of all wall states
- **Auto-Revert** — Disabling bypass immediately re-syncs all states to physical sensor readings
- **Per-Wall Output Pins** — Clean boolean outputs for each wall distributed to downstream plugins

## Installation

1. Copy `Wall_State_Manager_v2.qplug` to your Q-SYS Designer plugin directory:
   ```
   %USERPROFILE%\Documents\QSC\Q-Sys Designer\Plugins
   ```
2. Restart Q-SYS Designer
3. The plugin will appear in the **Schematic Library** under **Scripted Components**

## Configuration

### Properties

| Property | Type | Range | Default | Description |
|----------|------|-------|---------|-------------|
| `Number of Walls` | Integer | 1–10 | 2 | Number of wall sensor channels to create |

> **Note**: Changing this property regenerates all controls. Set it before wiring.

### Controls (Per Wall)

For each wall `i` (1 through N):

| Control | Type | Direction | Description |
|---------|------|-----------|-------------|
| `Sensor_i_In` | LED Indicator | Input | Physical sensor input for Wall i |
| `Wall_i_State` | Toggle Button | Both | Local UI state / manual override toggle |
| `Wall_i_Out` | LED Indicator | Output | Evaluated output for system use |

### Global Controls

| Control | Type | Direction | Description |
|---------|------|-----------|-------------|
| `Bypass_Enable` | Toggle Button | Both | Enable/disable manual override for all walls |

### Wiring

1. Connect physical wall sensor GPIO pins to `Sensor_1_In` through `Sensor_N_In`
2. Wire `Wall_1_Out` through `Wall_N_Out` to all downstream plugins:
   - PTZ Camera Controller
   - Video Router
   - UI Controller
   - ACPR Manager
   - Room State Manager

### Logic

| Mode | Behavior |
|------|----------|
| **Normal** (Bypass off) | Outputs follow sensor inputs with inverted logic. Toggles are greyed out. |
| **Bypass** (Bypass on) | Manual toggles directly control outputs. Sensors are ignored. |
| **Bypass → Off** | All states immediately re-sync to current sensor readings. |

## Migration from v1

If upgrading from v1:
1. Set `Number of Walls` to `2` in Properties (matches v1 behavior exactly)
2. Re-wire `Sensor_1_In`, `Sensor_2_In`, `Wall_1_Out`, `Wall_2_Out` — pin names are identical
3. The `Bypass_Enable` control works identically

## Author

**Dustin Bennett** — TEL Systems

## Version

v2.0.0
