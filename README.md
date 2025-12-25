# AGS Configuration Guide

This guide explains how to configure the Advanced Gauge System (AGS). It covers creating custom skins (`.asc` files) and managing color palettes (`.apd` files).

## Table of Contents
1. [Skin Configuration (.asc)](#skin-configuration-asc)
   - [File Structure](#file-structure)
   - [Global Settings](#global-settings)
   - [Texture Naming Conventions](#texture-naming-conventions)
   - [Standard Texture Checklist](#standard-texture-checklist)
   - [Gauge Settings](#gauge-settings)
   - [Symbol Clusters](#symbol-clusters)
   - [Valid Values (Enums)](#valid-values-enums)
2. [Palette System (.apd)](#palette-system-apd)
   - [UnifiedColor System](#unifiedcolor-system)
   - [Presets vs. Live Settings](#presets-vs-live-settings)

---

## Skin Configuration (.asc)

### File Structure
Skins are JSON files located in the `Skins` folder.

```json
{
  "SkinId": "AGS_S01",
  "Name": "Sport Carbon",
  "Author": "Khorio",
  "Version": "1.0",
  "EnabledGauges": "GT_RPM, GT_TURBO, GT_FUEL, GT_SPEEDO",
  "Scale": 1.0,
  "UseGlobalUnderlay": true,
  "RPM": { ... },
  "Speedo": { ... },
  "AdditionalSymbolClusters": [ ... ]
}
```

### Global Settings

| Property | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `SkinId` | String | "default_id" | The name of the `.ytd` texture dictionary to load. |
| `Scale` | Float | 1.0 | Global scale multiplier for the entire HUD. |
| `UseGlobalUnderlay` | Bool | false | Draws a single background sprite behind all gauges. |
| `GlobalUnderlayWidth` | Int | 1024 | Width of the global background texture. |
| `GlobalUnderlayHeight` | Int | 1024 | Height of the global background texture. |
| `GlobalUnderlayZIndex` | Int | 0 | 0 = Behind gauges, 1 = On top (glass effect). |
| `UseSharedUnderlay` | Bool | true | If true, all gauges use `UNDERLAY`. |
| `UseSharedLED` | Bool | true | Uses `LED_ON`/`LED_OFF` for all gauges. |
| `UseSharedNeedle` | Bool | true | Uses `NEEDLE` for all gauges. |

---

## Texture Naming Conventions

The system constructs names as `[Prefix]_[Layer]_[Suffix]`.

### 1. Base Layers
If `UseShared[Asset]` is **false**, names become unique:
*Prefixes: `RPM`, `SPEEDO`, `TURBO`, `FUEL`, `TEMP`, `OIL`*

| Layer | Shared Filename | Unique Filename (Example) |
| :--- | :--- | :--- |
| **Underlay** | `UNDERLAY` | `SPEEDO_UNDERLAY` |
| **Needle** | `NEEDLE` | `RPM_NEEDLE` |
| **LED On** | `LED_ON` | `TURBO_LED_ON` |
| **LED Off** | `LED_OFF` | `TURBO_LED_OFF` |

### 2. RPM Dynamic Faces
RPM faces swap based on engine capability:
- **Format:** `[Step]_[Layer]`
- **Example:** `8000_LINES_MAJOR`, `8000_NUMBERS`, `8000_REDLINE`.

### 3. Unit Suffixes & Night Mode
- **Speedo Numbers:** `SPEEDO_NUMBERS_KMH` or `SPEEDO_NUMBERS_MPH`.
- **Temp:** `TEMP_NUMBERS_C` or `TEMP_NUMBERS_F`.
- **Night Mode:** Any texture with `_NIGHT` (e.g., `NEEDLE_NIGHT`) will swap automatically at night.

---

## Standard Texture Checklist

### RPM Assets (Per Step: 7000, 8000, 9000, 10000)
- [ ] `[Step]_LINES_MAJOR`, `[Step]_LINES_MINOR`, `[Step]_NUMBERS`, `[Step]_REDLINE`
- [ ] `RPM_FILL`, `RPM_LABEL`
- [ ] `RPM_SPEED_LABEL_KMH`, `RPM_SPEED_LABEL_MPH`

### Speedometer Assets
- [ ] `SPEEDO_LINES_MAJOR`, `SPEEDO_LINES_MINOR`
- [ ] `SPEEDO_NUMBERS_KMH`, `SPEEDO_NUMBERS_MPH`

---

## Gauge Settings

### Common Properties
- **RelativePosition**: `{ "X": 0, "Y": 0 }` (Offset from center).
- **AngleMin / AngleMax**: Rotation degrees for the needle.
- **LEDThreshold**: 0.0 to 1.0 (Value at which the warning LED turns on).
- **DisableColoring**: Set to `true` to force original texture colors.

### RPM Logic
The system estimates Max RPM based on vehicle class multipliers:
- **Super/Bikes**: 200.0
- **Sport**: 185.0
- **Normal**: 160.0
- **Trucks/Industrial**: 80.0

```json
"RPM": {
  "Data": {
    "MaxRpm": 10000,
    "AvailableSteps": [7000, 8000, 9000, 10000],
    "SpeedDisplayScale": 1.0,
    "GearDisplayScale": 1.0
  },
  "Layout": {
    "GearPositionOffset": { "X": 24, "Y": 35 },
    "SpeedPositionOffset": { "X": -15, "Y": 42 }
  }
}
```

---

## Symbol Clusters

Warning icons can be positioned in a grid or a circle.

| Property | Description |
| :--- | :--- |
| `EnabledSymbols` | List of icons (e.g. `SY_A_ENGINE`, `SY_R_OIL`). |
| `UseRadialPositioning`| `true` for circular, `false` for grid. |
| `SymbolArcRadius` | Distance from center (Radial). |
| `Wrap` | Icons per row (Grid). |
| `SymbolSpacingX/Y` | Space between icons (Grid). |

---

## Valid Values (Enums)

### GAUGE_LAYER
`GL_UNDERLAY`, `GL_FILL`, `GL_LINES_MAJOR`, `GL_LINES_MINOR`, `GL_NEEDLE`, `GL_NUMBERS`, `GL_REDLINE`, `GL_LED`, `GL_ALL`

### DASH_SYMBOL
- **Amber**: `SY_A_ABS`, `SY_A_ENGINE`, `SY_A_FUEL`, `SY_A_TIREPRESSURE`
- **Red**: `SY_R_BATT`, `SY_R_OIL`, `SY_R_TEMP`, `SY_R_BRAKE_MAL`, `SY_R_DOORS`
- **Other**: `SY_G_TURN_L`, `SY_G_TURN_R`, `SY_B_HIGHBEAMS`

---

## Palette System (.apd)

### UnifiedColor System
Accepts GTA names (`"Red"`, `"HudColorGreen"`) or Hex codes (`"#FF0000"`).

### Presets vs. Live Settings
- **Palettes Folder**: These are **Read-Only** templates.
- **Live Settings**: Changes made in the in-game menu are saved to the "Living Instance" and do not overwrite your template files unless exported.

---
**Happy Skinning!**
