# Gauge Texture Checklist (Generics)

## 1. Global & Shared Elements
*These names typically apply if `isShared` is true or if they are universal components.*

- [ ] `UNDERLAY` (Base background)
- [ ] `GLOBAL_UNDERLAY`
- [ ] `NEEDLE`
- [ ] `FILL`
- [ ] `LED_ON`
- [ ] `LED_OFF`

---

## 2. Speedometer (`SPEEDO_`)
*Speedo textures are unit-aware for lines and redlines.*

- [ ] `SPEEDO_LINES_MAJOR_MPH` / `SPEEDO_LINES_MAJOR_KMH`
- [ ] `SPEEDO_LINES_MINOR_MPH` / `SPEEDO_LINES_MINOR_KMH`
- [ ] `SPEEDO_NUMBERS_MPH` / `SPEEDO_NUMBERS_KMH`
- [ ] `SPEEDO_LABEL_MPH` / `SPEEDO_LABEL_KMH`
- [ ] `SPEEDO_REDLINE_MPH` / `SPEEDO_REDLINE_KMH`

---

## 3. RPM / Tachometer (`[RPM]_`)
*RPM textures use the limit (e.g., 8000) as a prefix instead of "RPM".*

- [ ] `[RPM]_LINES_MAJOR` (e.g., `8000_LINES_MAJOR`)
- [ ] `[RPM]_LINES_MINOR`
- [ ] `[RPM]_NUMBERS`
- [ ] `[RPM]_REDLINE`
- [ ] `RPM_LABEL` (Generic unit label)

---

## 4. Temperature (`TEMP_`)
*Numbers and labels change based on Celsius or Fahrenheit.*

- [ ] `TEMP_NUMBERS_C` / `TEMP_NUMBERS_F`
- [ ] `TEMP_LABEL_C` / `TEMP_LABEL_F`
- [ ] `TEMP_LINES_MAJOR`
- [ ] `TEMP_LINES_MINOR`

---

## 5. Night Variants (`_NIGHT`)
*Add the `_NIGHT` suffix to any of the following to support night-mode illumination.*

- [ ] `..._LINES_MAJOR_NIGHT`
- [ ] `..._LINES_MINOR_NIGHT`
- [ ] `..._NUMBERS_NIGHT`
- [ ] `..._LABEL_NIGHT`
- [ ] `..._REDLINE_NIGHT`
- [ ] `..._FILL_NIGHT`
- [ ] `..._NEEDLE_NIGHT`
- [ ] `..._UNDERLAY_NIGHT`

---

## 🔄 Fallback Logic Reference
If a specific texture is missing, the system searches in this order:
1. **Specific:** `[Type]_[Unit]_NIGHT` (e.g., `SPEEDO_NUMBERS_KMH_NIGHT`)
2. **Day Fallback:** `[Type]_[Unit]` (e.g., `SPEEDO_NUMBERS_KMH`)
3. **Unit Fallback:** `[Type]_NIGHT` (e.g., `SPEEDO_NUMBERS_NIGHT`)
4. **Base:** `[Type]` (e.g., `SPEEDO_NUMBERS`)

# RPM Range Texture Checklist

## 1. 7000 RPM Configuration
*These textures use "7000" as the prefix. Note that the "RPM_" prefix is omitted for these specific layers.*

- [ ] `7000_LINES_MAJOR`
- [ ] `7000_LINES_MAJOR_NIGHT`
- [ ] `7000_LINES_MINOR`
- [ ] `7000_LINES_MINOR_NIGHT`
- [ ] `7000_NUMBERS`
- [ ] `7000_NUMBERS_NIGHT`
- [ ] `7000_REDLINE`
- [ ] `7000_REDLINE_NIGHT`

---

## 2. 9000 RPM Configuration
*These textures use "9000" as the prefix.*

- [ ] `9000_LINES_MAJOR`
- [ ] `9000_LINES_MAJOR_NIGHT`
- [ ] `9000_LINES_MINOR`
- [ ] `9000_LINES_MINOR_NIGHT`
- [ ] `9000_NUMBERS`
- [ ] `9000_NUMBERS_NIGHT`
- [ ] `9000_REDLINE`
- [ ] `9000_REDLINE_NIGHT`

---

## 3. General RPM Assets
*These assets remain constant regardless of the specific RPM limit/step prefix.*

- [ ] `RPM_LABEL` (The "RPM" or "x1000" text)
- [ ] `RPM_LABEL_NIGHT`
- [ ] `RPM_NEEDLE` (If not using a shared needle)
- [ ] `RPM_UNDERLAY` (If not using a shared underlay)
- [ ] `RPM_FILL` (Background illumination/fill)
- [ ] `RPM_FILL_NIGHT`

---

## 💡 Logic Reminder
* **RPM Step Prefix:** When generating names for `LINES_MAJOR`, `LINES_MINOR`, `NUMBERS`, or `REDLINE`, the system uses the RPM step (e.g., `7000`) as the only prefix to avoid double-prefixing like `RPM_7000_...`.
* **Unit Awareness:** By default, RPM `NUMBERS` and `LABELS` are unit-aware and can accept `_MPH` or `_KMH` suffixes if the gauge configuration requires it.

# Comprehensive Gauge Texture Checklist

This checklist covers the required texture names for all gauge types, including unit-specific variations and night-mode fallbacks as defined in `TextureResolver`.

## 1. Speedometer (`SPEEDO_`)
*Speedo textures are unit-aware for lines, numbers, and redlines.*

### Base / Day Textures
- [ ] `SPEEDO_LINES_MAJOR_MPH` / `SPEEDO_LINES_MAJOR_KMH`
- [ ] `SPEEDO_LINES_MINOR_MPH` / `SPEEDO_LINES_MINOR_KMH`
- [ ] `SPEEDO_NUMBERS_MPH` / `SPEEDO_NUMBERS_KMH`
- [ ] `SPEEDO_LABEL_MPH` / `SPEEDO_LABEL_KMH`
- [ ] `SPEEDO_REDLINE_MPH` / `SPEEDO_REDLINE_KMH`
- [ ] `SPEEDO_UNDERLAY` (If not shared)
- [ ] `SPEEDO_NEEDLE` (If not shared)
- [ ] `SPEEDO_FILL`

### Night Variants (`_NIGHT`)
- [ ] `SPEEDO_LINES_MAJOR_[UNIT]_NIGHT`
- [ ] `SPEEDO_LINES_MINOR_[UNIT]_NIGHT`
- [ ] `SPEEDO_NUMBERS_[UNIT]_NIGHT`
- [ ] `SPEEDO_LABEL_[UNIT]_NIGHT`
- [ ] `SPEEDO_REDLINE_[UNIT]_NIGHT`
- [ ] `SPEEDO_FILL_NIGHT`

---

## 2. RPM / Tachometer (`[RPM]_`)
*Specific layers use the RPM limit as the prefix. General layers use "RPM_".*

### Step-Specific Textures (e.g., 7000, 8000, 9000)
- [ ] `[STEP]_LINES_MAJOR`
- [ ] `[STEP]_LINES_MINOR`
- [ ] `[STEP]_NUMBERS`
- [ ] `[STEP]_REDLINE`
- [ ] Add `_NIGHT` suffix for all above

### General RPM Assets
- [ ] `RPM_LABEL` (Unit label)
- [ ] `RPM_LABEL_NIGHT`
- [ ] `RPM_FILL` / `RPM_FILL_NIGHT`
- [ ] `RPM_UNDERLAY` / `RPM_UNDERLAY_NIGHT`

---

## 3. Temperature (`TEMP_`)
*Numbers and labels change based on Celsius or Fahrenheit.*

### Unit-Specific
- [ ] `TEMP_NUMBERS_C` / `TEMP_NUMBERS_F`
- [ ] `TEMP_LABEL_C` / `TEMP_LABEL_F`
- [ ] Add `_NIGHT` suffix for all above

### Static Elements
- [ ] `TEMP_LINES_MAJOR` / `TEMP_LINES_MAJOR_NIGHT`
- [ ] `TEMP_LINES_MINOR` / `TEMP_LINES_MINOR_NIGHT`
- [ ] `TEMP_FILL` / `TEMP_FILL_NIGHT`
- [ ] `TEMP_UNDERLAY` / `TEMP_UNDERLAY_NIGHT`

---

## 4. Shared & Global Elements
*Used when `SkinConfig` flags like `UseSharedNeedle` are enabled.*

- [ ] `UNDERLAY` / `UNDERLAY_NIGHT`
- [ ] `GLOBAL_UNDERLAY`
- [ ] `NEEDLE` / `NEEDLE_NIGHT`
- [ ] `LED_ON` (Used for `GL_LED`)
- [ ] `LED_OFF`

---

## 5. Other Gauges (Fuel, Oil, etc.)
*Standard naming for `GAUGE_TYPE` values like `FUEL`, `OIL`, `TURBO`.*

- [ ] `[TYPE]_UNDERLAY`
- [ ] `[TYPE]_LINES_MAJOR`
- [ ] `[TYPE]_LINES_MINOR`
- [ ] `[TYPE]_NUMBERS`
- [ ] `[TYPE]_LABEL`
- [ ] `[TYPE]_FILL`
- [ ] Add `_NIGHT` suffix for all above
