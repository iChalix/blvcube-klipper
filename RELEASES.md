# Release Notes - BLV Cube Klipper Configuration

## Release v1.2.0 - "Precision Enhancement" (2025-11-08)

### What's New

This release focuses on improving print accuracy and quality through advanced calibration and compensation features.

#### Major Features

**🎯 Axis Twist Compensation**
- Automatically corrects for X-axis gantry twist across the build plate
- Uses 5 calibration points to map and compensate for mechanical flex
- Results in more consistent layer heights across large prints

**🔧 Z Backlash Compensation**
- Compensates for mechanical play in the Z-axis drive system
- Improves first layer consistency and overall Z-axis positioning accuracy
- Configured with precision measurements (0.00512mm)

**📐 Skew Correction**
- New "Califlower" skew correction profile
- Corrects for dimensional inaccuracies in X/Y movements
- Automatically applied during print start for improved part accuracy

**🚀 Improved Input Shaping**
- Significantly upgraded resonance compensation based on new calibration
- X-axis: 3x faster frequency response (41.667 Hz → 123.0 Hz)
- Y-axis: Nearly 2x faster frequency response (35.714 Hz → 61.0 Hz)
- Enables higher print speeds with reduced ringing and ghosting

#### Quality of Life Improvements

- **Prime Line**: Automatic nozzle priming before each print prevents initial under-extrusion
- **Updated Bed Mesh**: Fresh calibration data for optimal first layer adhesion
- **Fine-tuned Z-Offset**: Adjusted to -0.22mm for perfect first layer squish

### Performance Impact

- **Print Speed**: Can now print at higher speeds with better quality due to improved input shaping
- **Accuracy**: Better dimensional accuracy from skew correction
- **Reliability**: More consistent first layers and Z-positioning
- **Large Prints**: Improved quality on large prints thanks to axis twist compensation

### What You Need to Do

After updating to this version:

1. **Recalibrate Input Shaper** (recommended but optional):
   ```
   SHAPER_CALIBRATE
   SAVE_CONFIG
   ```

2. **Generate Skew Correction Profile** (required for skew correction):
   - Print a calibration object (square or rectangle with known dimensions)
   - Measure actual dimensions
   - Run skew correction calibration commands
   - See [Klipper Skew Correction Guide](https://www.klipper3d.org/Skew_Correction.html)

3. **Calibrate Axis Twist** (required for twist compensation):
   ```
   AXIS_TWIST_COMPENSATION_CALIBRATE
   SAVE_CONFIG
   ```

4. **Update Bed Mesh** (recommended):
   ```
   BED_MESH_CALIBRATE
   SAVE_CONFIG
   ```

5. **Adjust Z-Offset if needed**:
   - The configuration now uses -0.22mm
   - Fine-tune with a test print if necessary

---

## Release v1.1.0 - "Cartographer Integration" (2025-10-31)

### What's New

This release represents a major hardware and software upgrade with the integration of the Cartographer3D probe system.

#### Major Features

**🎮 Cartographer3D Probe System**
- Professional-grade touch and scanning probe replaces BLTouch
- Faster and more accurate bed probing
- Temperature-compensated readings for consistent results
- Dual-mode operation:
  - **Scan Mode**: Rapid surface profiling for quick surveys
  - **Touch Mode**: High-precision probing for accurate Z-offset

**📊 ADXL345 Accelerometer Integration**
- Enables automatic input shaper calibration
- Eliminates guesswork in resonance compensation setup
- Quick and accurate measurement of printer resonances

**🗺️ High-Resolution Bed Meshing**
- Upgraded from 5x5 to 20x20 probe grid (400 probe points!)
- Dramatically improved bed surface mapping
- Better first layer quality across the entire build plate
- Bicubic interpolation for smooth compensation

#### Hardware Changes

- Added Cartographer MCU as third control board
- Integrated ADXL345 accelerometer on Cartographer board
- Removed BLTouch probe hardware

#### Configuration Changes

- Probe offset: X=36mm, Y=0mm from nozzle
- Temperature reference: 25°C for probe compensation
- Enhanced mesh resolution and algorithms

### Performance Impact

- **Faster Bed Probing**: 20x20 mesh completes in comparable time to old 5x5 mesh
- **Better Accuracy**: Higher probe resolution and temperature compensation
- **Simplified Setup**: Automatic input shaper calibration vs manual testing

### What You Need to Do

This is a **hardware-dependent release**. To upgrade:

1. **Install Hardware**:
   - Mount Cartographer3D probe on toolhead
   - Connect Cartographer board via USB
   - Remove BLTouch hardware

2. **Flash Firmware**:
   - Flash Klipper firmware to Cartographer MCU
   - Update MCU serial path in configuration

3. **Calibrate System**:
   ```
   # Calibrate Cartographer scan model
   CARTOGRAPHER_CALIBRATE

   # Calibrate Cartographer touch model
   # Follow Cartographer documentation

   # Generate new bed mesh
   BED_MESH_CALIBRATE

   # Calibrate input shaper
   SHAPER_CALIBRATE

   SAVE_CONFIG
   ```

4. **Verify Probe Offset**:
   - Physical offset should be X=36mm, Y=0mm
   - Adjust if your mounting differs

---

## Release v1.0.0 - "Foundation" (2025-06-14)

### What's New

The inaugural release of the BLV Cube Klipper configuration - a complete, production-ready setup for running a custom CoreXY printer.

#### Core Features

**🏗️ Complete Printer Configuration**
- Full CoreXY kinematics setup for 310x310x400mm build volume
- Optimized for 300mm/s maximum velocity
- 3000mm/s² maximum acceleration

**🎛️ Triple MCU Architecture**
- Main MCU: X/Y motion and primary control
- Z Board: Dedicated Z-axis management
- Aux Board: Thermal system control
- Stable USB serial identification using `/dev/serial/by-id/`

**🔇 TMC2130 Silent Drivers**
- SPI-configured stepper drivers
- Sensorless homing on X and Y axes (no endstops needed!)
- StealthChop for quiet operation
- SpreadCycle for high-speed performance

**📏 Advanced Bed Leveling**
- Three-point Z-tilt adjustment
- Automatic bed tramming with three independent Z motors
- 5x5 bed mesh with bicubic interpolation
- BLTouch probe for precise measurements

**🌡️ Precise Thermal Control**
- PID-tuned hotend and heated bed
- Multiple temperature sensors (bed, chamber, MCU monitoring)
- Intelligent part cooling with variable speed
- Temperature-controlled hotend fan

**🎨 Quality Features**
- Pressure advance for improved corner quality
- Firmware retraction for cleaner travels
- Input shaping for resonance compensation
- Arc movement support (G2/G3 commands)

**📱 Fluidd Integration**
- Complete set of web interface macros
- Print control: START_PRINT, END_PRINT, PAUSE, RESUME, CANCEL
- Filament management: LOAD_FILAMENT, UNLOAD_FILAMENT
- Emergency controls and status monitoring

#### Documentation

- **README.md**: Complete setup guide with installation, calibration, and troubleshooting
- **CLAUDE.md**: AI assistant integration for configuration management

### What You Need to Do

For initial setup:

1. **Install Klipper** on your Raspberry Pi
2. **Flash Firmware** to all three Arduino Mega boards
3. **Update Serial Paths** in printer.cfg to match your system
4. **Run Initial Calibrations**:
   - PID tuning for hotend and bed
   - Z-offset calibration
   - Bed mesh generation
   - Z-tilt adjustment
   - Pressure advance tuning

Refer to README.md for detailed instructions.

---

## Version History Summary

| Version | Release Date | Key Feature | Impact |
|---------|--------------|-------------|--------|
| v1.2.0 | 2025-11-08 | Precision enhancements (axis twist, backlash, skew) | Higher accuracy, better large prints |
| v1.1.0 | 2025-10-31 | Cartographer3D probe integration | Faster probing, 20x20 mesh |
| v1.0.0 | 2025-06-14 | Initial complete configuration | Full printer functionality |

---

## Getting Help

- **Documentation**: See [README.md](README.md) for detailed information
- **Klipper Docs**: Visit [klipper3d.org](https://www.klipper3d.org/)
- **Cartographer**: Check [docs.cartographer3d.com](https://docs.cartographer3d.com/)
- **Issues**: Report problems or ask questions through your preferred channel

---

## Contributing

This configuration is actively maintained and improved based on real-world usage. Suggestions and improvements are welcome!
