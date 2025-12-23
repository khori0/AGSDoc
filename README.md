# AGS Skin Configuration Guide

This guide provides the necessary technical specifications for creating custom gauge skins for the **AGS HUD system**. Skins are defined using JSON logic stored in `.asc` files.

---

## 1. File Structure & Location
To ensure the HUD system recognizes your custom skin, adhere to the following file standards:

* **File Extension:** `.asc` (AGS Skin Config)
* **Directory:** `GTAV/plugins/AGS/Skins/`
* **Format:** Standard JSON

---

## 2. Texture Naming Convention
The system dynamically loads textures from your `.ytd` (Texture Dictionary) based on the following naming patterns:

* **[ID]:** The `SkinId` defined in your config (e.g., `AGS_S01`).
* **[PREFIX]:** The gauge type (e.g., `RPM`, `TURBO`, `FUEL`, `TEMP`, `OIL`).

### Layer Naming Table
| Layer | Texture Name Format | Notes |
| :--- | :--- | :--- |
| **Underlay** | `[PREFIX]_UNDERLAY` | If `UseSharedUnderlay` is true, simply name it `UNDERLAY`. |
| **Fill** | `[PREFIX]_FILL` | Background behind lines but above the underlay. |
| **Major Lines** | `[PREFIX]_LINES_MAJOR` | The primary tick marks. |
| **Minor Lines** | `[PREFIX]_LINES_MINOR` | The secondary/smaller tick marks. |
| **Needle** | `[PREFIX]_NEEDLE` | The moving pointer/indicator. |
| **Numbers** | `[PREFIX]_NUMBERS` | Static numerical values on the face. |
| **Redline** | `[PREFIX]_REDLINE` | The high-RPM danger zone indicator. |

> **Note on RPM Special Case:** The RPM gauge is dynamic. You must provide textures prefixed with the RPM step (e.g., `7000_LINES_MAJOR`, `9000_NUMBERS`).

---

## 3. Configuration Reference

### Root Object
*The top-level settings for the entire skin.*

| Property | Type | Default | Description |
| :--- | :--- | :--- | :--- |
| `SkinId` | String | "default" | The name of the corresponding `.ytd` file. |
| `Name` | String | "default" | The name shown in the in-game menu. |
| `Author` | String | "default" | Creator's name. |
| `Version` | String | "1.0" | Skin version. |
| `Scale` | Float | 1.0 | Global scale multiplier for the entire HUD. |
| `EnabledGauges` | Flag | "GT_ALL" | Which gauges to render (see Enums). |
| `UseGlobalUnderlay` | Bool | false | Draws one large background behind all gauges. |
| `UseSharedUnderlay` | Bool | true | All gauges share a single `UNDERLAY` texture. |
| `GlobalUnderlayWidth` | Int | 1024 | Width of the global background texture. |
| `GlobalUnderlayHeight` | Int | 1024 | Height of the global background texture. |

### Gauge Settings
*Settings applied to individual gauge blocks (RPM, Turbo, etc).*

| Property | Type | Description |
| :--- | :--- | :--- |
| `RelativePosition` | Point | `{ "X": 0, "Y": 0 }` offset from the HUD center. |
| `Scale` | Float | Local scale multiplier for this specific gauge. |
| `AngleMin` | Float | Rotation angle (degrees) at the minimum value. |
| `AngleMax` | Float | Rotation angle (degrees) at the maximum value. |
| `GaugeLayers` | Flag | Which layers to draw (e.g., `"GL_NEEDLE, GL_NUMBERS"`). |
| `DisableColoring` | Bool | If true, ignores user color palettes. |
| `Symbols` | Object | A SymbolLayout object attached to this gauge. |

---

## 4. Valid Values (Enums)

### GAUGE_TYPE (Flags)
*Combine using commas (e.g., `"GT_RPM, GT_TURBO"`)*
* `GT_NONE`
* `GT_RPM`
* `GT_TURBO`
* `GT_FUEL`
* `GT_TEMP`
* `GT_OIL`
* `GT_ALL`

### GAUGE_LAYER (Flags)
* `GL_NONE`
* `GL_UNDERLAY`
* `GL_FILL`
* `GL_LINES_MAJOR`
* `GL_LINES_MINOR`
* `GL_NEEDLE`
* `GL_NUMBERS`
* `GL_REDLINE`
* `GL_ALL`

### DASH_SYMBOL (Warning Lights)
| Category | Symbols |
| :--- | :--- |
| **Amber (Warning)** | `SY_A_ABS`, `SY_A_ASR`, `SY_A_ENGINE`, `SY_A_FOGL`, `SY_A_FUEL`, `SY_A_TIREPRESSURE`, `SY_A_DLIGHT` |
| **Red (Critical)** | `SY_R_BATT`, `SY_R_BRAKE_MAL`, `SY_R_DOOR`, `SY_R_DOORS`, `SY_R_HOOD`, `SY_R_TRUNK`, `SY_R_OIL`, `SY_R_PBRAKE`, `SY_R_TEMP` |
| **Green/Blue (Info)** | `SY_G_DAYTIME`, `SY_G_HEADLIGHTS`, `SY_G_TURN_L`, `SY_G_TURN_R`, `SY_B_HIGHBEAMS` |

---

## 5. Example Configuration
```json
{
  "SkinId": "AGS_S01",
  "Name": "Sport Carbon",
  "Author": "Khorio",
  "Version": "1.0",
  "EnabledGauges": "GT_RPM, GT_TURBO, GT_FUEL, GT_TEMP",
  "Scale": 1.0,
  "UseGlobalUnderlay": true,
  "GlobalUnderlayWidth": 1024,
  "GlobalUnderlayHeight": 512,
  "UseSharedUnderlay": true,

  "RPM": {
    "GaugeLayers": "GL_ALL",
    "Scale": 0.85,
    "RelativePosition": { "X": -172, "Y": 0 },
    "AngleMin": -180.0,
    "AngleMax": 90.0,
    "Layout": {
      "GearPositionOffset": { "X": 24, "Y": 35 },
      "SpeedPositionOffset": { "X": -15, "Y": 42 }
    },
    "Data": {
      "MinRpm": 5000,
      "MaxRpm": 10000,
      "AvailableSteps": [ 5000, 7000, 8000, 9000, 10000 ],
      "SpeedDisplayScale": 0.35,
      "GearDisplayScale": 0.25
    },
    "Symbols": {
      "SymbolClusterOffset": { "X": 0, "Y": -20 },
      "SymbolSpacingX": 50,
      "EnabledSymbols": [ "SY_G_TURN_L", "SY_G_TURN_R" ]
    }
  },

  "Turbo": {
    "GaugeLayers": "GL_UNDERLAY, GL_LINES_MAJOR, GL_NEEDLE, GL_NUMBERS",
    "RelativePosition": { "X": 15, "Y": -142 },
    "AngleMin": -60.0,
    "AngleMax": 60.0,
    "Scale": 0.375
  },

  "Fuel": {
    "RelativePosition": { "X": 275, "Y": -142 },
    "AngleMin": 150.0,
    "AngleMax": 30.0,
    "Scale": 0.375,
    "Symbols": {
      "EnabledSymbols": ["SY_A_FUEL"],
      "SymbolClusterOffset": { "X": -60, "Y": 0 }
    }
  },

  "AdditionalSymbolClusters": [
    {
      "UseRadialPositioning": false,
      "SymbolClusterOffset": { "X": 80, "Y": 83 },
      "SymbolSpacingX": 5,
      "Wrap": 5,
      "SymbolScale": 0.65,
      "EnabledSymbols": [
        "SY_R_BATT",
        "SY_G_HEADLIGHTS",
        "SY_B_HIGHBEAMS",
        "SY_A_ENGINE",
        "SY_A_ABS",
        "SY_R_PBRAKE"
      ]
    }
  ]
}
