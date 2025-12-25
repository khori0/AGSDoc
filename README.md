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

---

## Skin Configuration (.asc)

### File Structure
Skins define the layout, textures, and behavior of the HUD. They are JSON files located in the `Skins` folder.

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
| `Name` | String | "default_name" | Display name in the menu. |
| `Author` | String | "default_author" | Creator name. |
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

### 1. Base Layers
If `UseShared[Asset]` is set to **false**, names require the gauge prefix. 
*Prefixes: `RPM`, `SPEEDO`, `TURBO`, `FUEL`, `TEMP`, `OIL`*

| Layer | Shared Filename | Unique Filename (Example) |
| :--- | :--- | :--- |
| **Underlay** | `UNDERLAY` | `SPEEDO_UNDERLAY` |
| **Needle** | `NEEDLE` | `RPM_NEEDLE` |
| **LED On** | `LED_ON` | `TURBO_LED_ON` |
| **LED Off** | `LED_OFF` | `TURBO_LED_OFF` |

### 2. Standard Layers
- `[PREFIX]_LINES_MAJOR`, `[PREFIX]_LINES_MINOR`, `[PREFIX]_FILL`

### 3. RPM Dynamic Faces
- **Format:** `[Step]_[Layer]`
- **Example:** `8000_LINES_MAJOR`, `8000_NUMBERS`, `8000_REDLINE`.

### 4. Unit Suffixes & Night Mode
- **Suffixes:** `SPEEDO_NUMBERS_KMH`, `TEMP_NUMBERS_C`, etc.
- **Night Mode:** Suffix any texture with `_NIGHT` to trigger automatic swapping.

---

## Standard Texture Checklist

### RPM Assets (Per Step)
- [ ] `[Step]_LINES_MAJOR`, `[Step]_LINES_MINOR`, `[Step]_NUMBERS`, `[Step]_REDLINE`
- [ ] `RPM_FILL`, `RPM_LABEL`, `RPM_SPEED_LABEL_KMH`, `RPM_SPEED_LABEL_MPH`

### Speedometer Assets
- [ ] `SPEEDO_LINES_MAJOR`, `SPEEDO_LINES_MINOR`
- [ ] `SPEEDO_NUMBERS_KMH`, `SPEEDO_NUMBERS_MPH`

### Utility Gauges
- [ ] `[PREFIX]_LINES_MAJOR`, `[PREFIX]_LINES_MINOR`, `[PREFIX]_FILL`
- [ ] `TEMP_NUMBERS_C`, `TEMP_NUMBERS_F`, `TEMP_REDLINE_C`, `TEMP_REDLINE_F`

---

## Gauge Settings

### Common Properties
- **RelativePosition**: `{ "X": 0, "Y": 0 }`
- **AngleMin / AngleMax**: Rotation degrees.
- **LEDThreshold**: 0.0 to 1.0.
- **DisableColoring**: `true` to bypass the Palette system.

### RPM Logic
The system estimates Max RPM based on vehicle class (Super: 200, Sport: 185, Normal: 160, Trucks: 80).

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

Used for warning lights. Can be defined inside a Gauge or in `AdditionalSymbolClusters`.

| Property | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `EnabledSymbols` | List | [`SY_NONE`] | List of [DASH_SYMBOL](#dash_symbol) to include. |
| `UseRadialPositioning` | Bool | false | `true` = Circular arc, `false` = Grid layout. |
| `SymbolClusterOffset` | Point | 0,0 | Global offset for the cluster. |
| `SymbolScale` | Float | 0.5 | Scaling of the icons. |
| **Grid Properties** | | | |
| `SymbolSpacingX / Y` | Int | 0 | Spacing between icons in a grid. |
| `Wrap` | Int | 2 | Icons per row. |
| **Radial Properties** | | | |
| `SymbolArcCenterOffset`| Point | 0,0 | Center of the arc relative to cluster offset. |
| `SymbolArcRadius` | Float | 150.0 | Distance from center. |
| `SymbolArcAngleStart` | Float | -135.0 | Start angle of the arc in degrees. |
| `SymbolArcAngleEnd` | Float | 45.0 | End angle of the arc in degrees. |

---

## Valid Values (Enums)

#### GAUGE_TYPE
`GT_NONE`, `GT_RPM`, `GT_SPEEDO`, `GT_TURBO`, `GT_FUEL`, `GT_TEMP`, `GT_OIL`, `GT_ALL`

#### GAUGE_LAYER
`GL_UNDERLAY`, `GL_FILL`, `GL_LINES_MAJOR`, `GL_LINES_MINOR`, `GL_NEEDLE`, `GL_NUMBERS`, `GL_REDLINE`, `GL_LED`, `GL_ALL`

#### DASH_SYMBOL
- **Amber:** `SY_A_ABS`, `SY_A_ASR`, `SY_A_ENGINE`, `SY_A_FOGL`, `SY_A_FUEL`, `SY_A_TIREPRESSURE`, `SY_A_DLIGHT`
- **Blue:** `SY_B_HIGHBEAMS`
- **Green:** `SY_G_DAYTIME`, `SY_G_FOGL`, `SY_G_HEADLIGHTS`, `SY_G_TURN_L`, `SY_G_TURN_R`
- **Red:** `SY_R_BATT`, `SY_R_BRAKE_MAL`, `SY_R_DOOR`, `SY_R_DOORS`, `SY_R_HOOD`, `SY_R_OIL`, `SY_R_PBRAKE`, `SY_R_TEMP`, `SY_R_TRUNK`

---

## Palette System (.apd)

### UnifiedColor System
Accepts GTA HUD colors (`"Red"`, `"HudColorGreen"`) or Hex codes (`"#FF0000"`).

### Palette Structure
```json
{
  "Name": "Neon Night",
  "Primary": "White",
  "Accent": "#00FFFF",
  "Warning": "Red",
  "Background": "#000000",
  "NeedleOverride": "Orange",
  "DigitOverride": "White"
}
```

---
**Happy Skinning!**
