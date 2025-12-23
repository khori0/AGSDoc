### AGS Skinning: Quick Start Checklist

If you are ready to start building your first skin, follow these high-level steps to ensure everything connects correctly.

---

#### 1. Asset Preparation
* **The Texture Dictionary:** Create a `.ytd` file named after your `SkinId` (e.g., `AGS_S01.ytd`).
* **The Textures:** Ensure your images follow the naming convention (e.g., `RPM_NEEDLE`, `FUEL_LINES_MAJOR`).
* **RPM Variation:** If you want different faces for different cars, create textures like `7000_NUMBERS` and `9000_NUMBERS`.

#### 2. Configuration Setup
* **The File:** Create a text file and save it as `YourSkinName.asc`.
* **The Root:** Fill in your `SkinId`, `Author`, and `Name`.
* **The Placement:** Move this file to `GTAV/plugins/AGS/Skins/`.

#### 3. Key Parameters to Tune
| Step | Action | Logic |
| :--- | :--- | :--- |
| **Step A** | **Positioning** | Use `RelativePosition` to move gauges. `{0,0}` is the HUD center. |
| **Step B** | **Rotation** | Set `AngleMin` and `AngleMax` to match the "0" and "Max" marks on your texture. |
| **Step C** | **Visibility** | Use `GaugeLayers` to hide parts you didn't draw (e.g., remove `GL_REDLINE` if it's baked into your numbers). |

#### 4. Troubleshooting Common Issues
* **Invisible Gauge:** Check if `EnabledGauges` includes that specific gauge type.
* **White Box:** This usually means the `.ytd` name doesn't match the `SkinId` in the `.asc` file, or a texture name is misspelled.
* **Needle Offset:** If the needle rotates from the wrong spot, ensure your needle texture is centered in its own image file.
