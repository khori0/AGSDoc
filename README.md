# AGS Configuration Guide

This guide explains how to configure the Advanced Gauge System (AGS). It covers creating custom skins (`.asc` files) and managing color palettes (`.apd` files).

## Table of Contents
1. [Skin Configuration (.asc)](#skin-configuration-asc)
   - [File Structure](#file-structure)
   - [Global Settings](#global-settings)
   - [Gauge Settings](#gauge-settings)
   - [Symbol Clusters](#symbol-clusters)
   - [Valid Values (Enums)](#valid-values-enums)
2. [Palette System (.apd)](#palette-system-apd)
   - [UnifiedColor System](#unifiedcolor-system)
   - [Palette Structure](#palette-structure)
   - [Presets vs. Live Settings](#presets-vs-live-settings)

---

## Skin Configuration (.asc)

Skins define the layout, textures, and behavior of the HUD. They are JSON files located in the `Skins` folder.

### File Structure

A basic skin file looks like this:

```json
{
  "SkinId": "AGS_S01",
  "Name": "Sport Carbon",
  "Author": "Khorio",
  "Version": "1.0",
  "EnabledGauges": "GT_RPM, GT_TURBO, GT_FUEL",
  "Scale": 1.0,
  "UseGlobalUnderlay": true,
  "RPM": { ... },
  "Speedo": { ... }
}
```

### Global Settings

These settings apply to the entire HUD.

| Property | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `SkinId` | String | "default_id" | The name of the `.ytd` texture dictionary to load. |
| `Name` | String | "default_name" | Display name in the menu. |
| `Author` | String | "default_author" | Creator name. |
| `Version` | String | "default_version" | Skin version. |
| `EnabledGauges` | [GAUGE_TYPE](#gauge_type) | "GT_ALL" | Which gauges to render. Combine flags with commas. |
| `Scale` | Float | 1.0 | Global scale multiplier for the entire HUD. |
| `UseGlobalUnderlay` | Bool | false | If true, draws a single large background sprite behind all gauges. |
| `GlobalUnderlayWidth` | Int | 1024 | Width of the global background texture. |
| `GlobalUnderlayHeight` | Int | 1024 | Height of the global background texture. |
| `GlobalUnderlayZIndex` | Int | 0 | 0 = Behind gauges, 1 = On top (glass effect). |
| `UseSharedUnderlay` | Bool | true | If true, all gauges look for a generic `UNDERLAY` texture. |
| `UseSharedLED` | Bool | true | If true, uses a shared LED texture. |
| `UseSharedNeedle` | Bool | true | If true, uses a shared needle texture. |

### Gauge Settings

Each gauge (`RPM`, `Speedo`, `Turbo`, `Fuel`, `Temp`, `Oil`) has its own configuration block.

**Common Properties:**

| Property | Type | Description |
| :--- | :--- | :--- |
| `RelativePosition` | Point | `{ "X": 0, "Y": 0 }` offset from the HUD center. |
| `Scale` | Float | Local scale multiplier for this specific gauge. |
| `AngleMin` | Float | The rotation angle (degrees) at the gauge's minimum value. |
| `AngleMax` | Float | The rotation angle (degrees) at the gauge's maximum value. |
| `GaugeLayers` | [GAUGE_LAYER](#gauge_layer) | Which layers to draw (e.g., `"GL_NEEDLE, GL_NUMBERS"`). |
| `DisableColoring` | Bool | If true, the gauge ignores user color palettes. |
| `HasNightTimeVariant` | Bool | If true, looks for `_NIGHT` suffixed textures at night. |
| `LEDThreshold` | Float | Value (0.0-1.0) at which the LED layer activates. |
| `Symbols` | [SymbolLayout](#symbol-clusters) | A symbol cluster attached specifically to this gauge. |

---

#### RPM Specifics

The `RPM` block uses `Data` for engine-logic and `Layout` for digital element positioning.

```json
"RPM": {
  "Data": {
    "MinRpm": 7000,
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

#### Speedo Specifics

The `Speedo` block handles the physical scaling of the speedometer needle.

| Property | Type | Description |
| :--- | :--- | :--- |
| `MaxSpeedValueInMetersPerSecond` | Float | The real-world speed that represents the end of the gauge dial. |

> **Tip:** To convert km/h to m/s, divide by 3.6. For example, a 300 km/h dial would be approximately 83.33.

```json
"Speedo": {
  "MaxSpeedValueInMetersPerSecond": 83.33,
  "RelativePosition": { "X": 450, "Y": 0 },
  "AngleMin": -140.0,
  "AngleMax": 140.0
}
```

---

### Symbol Clusters

Used for warning lights. Can be defined inside a Gauge or in `AdditionalSymbolClusters`.

| Property | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `EnabledSymbols` | Enum[] | [] | List of [DASH_SYMBOL](#dash_symbol) to include. |
| `SymbolClusterOffset` | Point | 0,0 | Position offset. |
| `SymbolScale` | Float | 0.5 | Size of the icons. |
| `UseRadialPositioning` | Bool | false | `true` = Circle, `false` = Grid. |
| `SymbolSpacingX` | Int | 0 | Horizontal space (Grid mode). |
| `SymbolSpacingY` | Int | 0 | Vertical space (Grid mode). |
| `Wrap` | Int | 2 | Icons per row (Grid mode). |
| `SymbolArcRadius` | Float | 150.0 | Distance from center (Radial mode). |

### Valid Values (Enums)

#### GAUGE_TYPE
Combine with commas (e.g., `"GT_RPM, GT_TURBO"`).
* `GT_NONE`, `GT_RPM`, `GT_SPEEDO`, `GT_TURBO`, `GT_FUEL`, `GT_TEMP`, `GT_OIL`, `GT_ALL`

#### GAUGE_LAYER
* `GL_NONE`, `GL_UNDERLAY`, `GL_FILL`, `GL_LINES_MAJOR`, `GL_LINES_MINOR`, `GL_NEEDLE`, `GL_NUMBERS`, `GL_REDLINE`, `GL_LED`, `GL_ALL`

#### DASH_SYMBOL
* **Amber:** `SY_A_ABS`, `SY_A_ASR`, `SY_A_ENGINE`, `SY_A_FOGL`, `SY_A_FUEL`, `SY_A_TIREPRESSURE`, `SY_A_DLIGHT`
* **Red:** `SY_R_BATT`, `SY_R_BRAKE_MAL`, `SY_R_DOOR`, `SY_R_DOORS`, `SY_R_HOOD`, `SY_R_TRUNK`, `SY_R_OIL`, `SY_R_PBRAKE`, `SY_R_TEMP`
* **Green/Blue:** `SY_G_DAYTIME`, `SY_G_HEADLIGHTS`, `SY_G_TURN_L`, `SY_G_TURN_R`, `SY_B_HIGHBEAMS`

---

## Palette System (.apd)

Palettes control the colors of the HUD elements. They are stored in `.apd` (AGS Palette Data) files.

### UnifiedColor System

The configuration uses a `UnifiedColor` type that accepts either a standard GTA HUD color name OR a custom Hex code.

* **Enum Name:** `"Red"`, `"Blue"`, `"White"`, `"HudColorGreen"`
* **Hex Code:** `"#FF0000"`, `"#00FF00"`, `"#RRGGBB"`

### Palette Structure

```json
{
  "Name": "Neon Night",
  "Primary": "White",
  "Accent": "#00FFFF",
  "Warning": "Red",
  "Background": "#000000",
  "NeedleOverride": "Orange",
  "DigitOverride": "White",
  "LabelOverride": null
}
```

### Presets vs. Live Settings

* **Presets Folder:** Files in the `Palettes` directory are **read-only templates**.
* **Live Settings:** The game maintains a "Living Instance" (Master/Worker) in `HUDSettings`.
* **Modifying:** Changes via menu sliders affect the **Live Instance**.
* **Saving:** To keep custom colors permanently, export them as a new Preset from the menu.

---
**Happy Skinning!**
