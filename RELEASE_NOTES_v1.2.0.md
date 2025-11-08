# Release v1.2.0 - "Precision Enhancement"

**Release Date:** 2025-11-08

---

## 🎯 What's New

This release focuses on improving print accuracy and quality through advanced calibration and compensation features.

### Major Features

#### 🎯 Axis Twist Compensation
- Automatically corrects for X-axis gantry twist across the build plate
- Uses 5 calibration points to map and compensate for mechanical flex
- Results in more consistent layer heights across large prints

#### 🔧 Z Backlash Compensation
- Compensates for mechanical play in the Z-axis drive system
- Improves first layer consistency and overall Z-axis positioning accuracy
- Configured with precision measurements (0.00512mm)

#### 📐 Skew Correction
- New "Califlower" skew correction profile
- Corrects for dimensional inaccuracies in X/Y movements
- Automatically applied during print start for improved part accuracy

#### 🚀 Improved Input Shaping
- Significantly upgraded resonance compensation based on new calibration
- **X-axis:** 3hump_ei shaper @ **123.0 Hz** (improved from 41.667 Hz) - **3x faster!**
- **Y-axis:** zv shaper @ **61.0 Hz** (improved from 35.714 Hz) - **nearly 2x faster!**
- Enables higher print speeds with reduced ringing and ghosting

### Quality of Life Improvements

- ✨ **Prime Line:** Automatic nozzle priming before each print prevents initial under-extrusion
- 🗺️ **Updated Bed Mesh:** Fresh calibration data for optimal first layer adhesion
- 🎚️ **Fine-tuned Z-Offset:** Adjusted to -0.22mm for perfect first layer squish

---

## 📊 Performance Impact

| Aspect | Improvement |
|--------|-------------|
| **Print Speed** | Higher speeds possible with better quality due to improved input shaping |
| **Accuracy** | Better dimensional accuracy from skew correction |
| **Reliability** | More consistent first layers and Z-positioning |
| **Large Prints** | Improved quality on large prints thanks to axis twist compensation |

---

## 🔧 What You Need to Do

After updating to this version:

### Required Calibrations

1. **Generate Skew Correction Profile** (required for skew correction):
   - Print a calibration object (square or rectangle with known dimensions)
   - Measure actual dimensions
   - Run skew correction calibration commands
   - See [Klipper Skew Correction Guide](https://www.klipper3d.org/Skew_Correction.html)

2. **Calibrate Axis Twist** (required for twist compensation):
   ```gcode
   AXIS_TWIST_COMPENSATION_CALIBRATE
   SAVE_CONFIG
   ```

### Recommended Calibrations

3. **Recalibrate Input Shaper** (recommended):
   ```gcode
   SHAPER_CALIBRATE
   SAVE_CONFIG
   ```

4. **Update Bed Mesh** (recommended):
   ```gcode
   BED_MESH_CALIBRATE
   SAVE_CONFIG
   ```

5. **Adjust Z-Offset if needed**:
   - The configuration now uses -0.22mm
   - Fine-tune with a test print if necessary

---

## 📋 Detailed Changes

### Added
- **Axis Twist Compensation**: Automatic compensation for X-axis twist across the build plate
  - Configured with 5 compensation points from X=40 to X=234
  - Automatically applies correction values during printing
- **Z Backlash Estimation**: Z-axis backlash compensation with 0.00512mm correction
- **Resonance Testing**: Comprehensive five-point testing pattern for input shaper calibration
- **Skew Correction**: Profile "Califlower" with XY skew correction value
- **Prime Line**: Nozzle prime line at print start (10mm of filament)

### Changed
- **Input Shaper Configuration**: Updated to new resonance test results
  - X-axis: 3hump_ei shaper @ 123.0 Hz
  - Y-axis: zv shaper @ 61.0 Hz
- **Z-Offset Calibration**: Updated from 0 to -0.22mm
- **Bed Mesh**: Completely recalibrated with latest probe readings

### Technical Details
- Input shaper improvements allow for faster printing with reduced ringing/ghosting
- Axis twist compensation addresses mechanical flex in the X-axis gantry
- Z backlash compensation accounts for mechanical play in the Z-axis drive system

---

## 🆙 Upgrade from v1.1.0

1. **Backup your current configuration**
2. **Pull the latest changes** from the repository
3. **Run required calibrations** (see above)
4. **Test with a small print** before running production jobs

---

## 📚 Documentation

- **[RELEASES.md](RELEASES.md)** - Detailed release notes for all versions
- **[CHANGELOG.md](CHANGELOG.md)** - Technical changelog following semantic versioning
- **[README.md](README.md)** - Complete setup and usage guide

---

## 🐛 Known Issues

None at this time.

---

## 💡 Support

For issues or questions:
- Review the [README.md](README.md) for detailed configuration information
- Check [Klipper documentation](https://www.klipper3d.org/) for general Klipper help
- Consult [Cartographer3D documentation](https://docs.cartographer3d.com/) for probe-specific questions

---

## 🙏 Credits

Configuration improvements based on real-world testing and calibration.

---

**Full Changelog**: https://github.com/iChalix/blvcube-klipper/compare/v1.1.0...v1.2.0
