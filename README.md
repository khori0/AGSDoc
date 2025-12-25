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
  "Turbo": { ... }
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
| `UseSharedUnderlay` | Bool | true | If true, all gauges look for a generic `UNDERLAY` texture. If false, they look for `RPM_UNDERLAY`, `TURBO_UNDERLAY`, etc. |
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
| `DisableColoring` | Bool | If true, the gauge ignores user color palettes (useful for photo-real skins). |
| `HasNightTimeVariant` | Bool | If true, the system looks for `_NIGHT` suffixed textures at night. |
| `LEDThreshold` | Float | Value (0.0-1.0) at which the LED layer activates (default 0.75). |
| `Symbols` | [SymbolLayout](#symbol-clusters) | A symbol cluster attached specifically to this gauge. |

**RPM Specifics:**

The `RPM` block has extra `Data` and `Layout` objects.

```json
"RPM": {
  "Data": {
    "MinRpm": 7000,             // Lowest RPM face available
    "MaxRpm": 10000,            // Highest RPM face available
    "AvailableSteps": [7000, 9000, 10000], // List of texture prefixes available
    "SpeedDisplayScale": 1.0,   // Scale of the digital speed text
    "GearDisplayScale": 1.0     // Scale of the digital gear text
  },
  "Layout": {
    "GearPositionOffset": { "X": 24, "Y": 35 },
    "SpeedPositionOffset": { "X": -15, "Y": 42 }
  }
}
```

### Symbol Clusters

Used for warning lights (Check Engine, Turn Signals, etc.). Can be defined inside a Gauge's `Symbols` property or in the root `AdditionalSymbolClusters` list.

| Property | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `EnabledSymbols` | [DASH_SYMBOL](#dash_symbol)[] | [] | List of symbols to include. |
| `SymbolClusterOffset` | Point | 0,0 | Position offset. |
| `SymbolScale` | Float | 0.5 | Size of the icons. |
| `UseRadialPositioning` | Bool | false | `true` = Circle arrangement, `false` = Grid. |
| `SymbolSpacingX` | Int | 0 | Horizontal space between icons (Grid mode). |
| `SymbolSpacingY` | Int | 0 | Vertical space between icons (Grid mode). |
| `Wrap` | Int | 2 | How many icons per row before wrapping (Grid mode). |
| `SymbolArcRadius` | Float | 150.0 | Distance from center (Radial mode). |
| `SymbolArcAngleStart` | Float | -135.0 | Start angle (Radial mode). |
| `SymbolArcAngleEnd` | Float | 45.0 | End angle (Radial mode). |

### Valid Values (Enums)

#### GAUGE_TYPE
Combine with commas (e.g., `"GT_RPM, GT_TURBO"`).
* `GT_NONE`
* `GT_RPM`
* `GT_SPEEDO`
* `GT_TURBO`
* `GT_FUEL`
* `GT_TEMP`
* `GT_OIL`
* `GT_ALL`

#### GAUGE_LAYER
Controls which image layers are visible.
* `GL_NONE`
* `GL_UNDERLAY`
* `GL_FILL`
* `GL_LINES_MAJOR`
* `GL_LINES_MINOR`
* `GL_NEEDLE`
* `GL_NUMBERS`
* `GL_REDLINE`
* `GL_LED`
* `GL_ALL`

#### DASH_SYMBOL
Valid strings for `EnabledSymbols`.

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

A palette file defines core colors and optional overrides.

```json
{
  "Name": "Neon Night",
  
  // Core Colors (Required)
  "Primary": "White",       // Used for Minor Lines, Labels (fallback)
  "Accent": "#00FFFF",      // Used for Major Lines, Needle (fallback)
  "Warning": "Red",         // Used for Redlines
  "Background": "#000000",  // Used for Underlay

  // Overrides (Optional - specific control)
  "NeedleOverride": "Orange",
  "DigitOverride": "White",
  "LabelOverride": null     // null means use Primary
}
```

### Presets vs. Live Settings

* **Presets Folder:** Files in the `Palettes` directory are **read-only templates**. You cannot modify them directly from the game menu.
* **Live Settings:** The game maintains a "Living Instance" of the palette in `HUDSettings`.
* **Modifying:** When you change colors using the in-game menu sliders, you are modifying the **Live Instance**, not the file on disk.
* **Saving:** To save your custom colors, you must export them as a new Preset from the menu.

**Note:** If you modify a palette via the menu, the system may append `(Modified)` to the name in the UI to indicate it no longer matches the saved preset.
