# Changelog

All notable changes to the BLV Cube Klipper configuration will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.2.0] - 2025-11-08

### Added
- **Axis Twist Compensation**: Added automatic compensation for X-axis twist across the build plate
  - Configured with 5 compensation points from X=40 to X=234
  - Automatically applies correction values during printing
- **Z Backlash Estimation**: Implemented Z-axis backlash compensation
  - Configured with 0.00512mm backlash correction
  - Improves first layer accuracy and Z-axis positioning
- **Resonance Testing**: Added comprehensive resonance test configuration
  - Five-point testing pattern covering corners and center of build plate
  - Enables automatic input shaper calibration
- **Skew Correction**: Implemented skew correction profile "Califlower"
  - XY skew correction: 0.0026023516475121313
  - Automatically loaded during print start sequence
  - Improves dimensional accuracy
- **Prime Line**: Added nozzle prime line at print start
  - Primes 10mm of filament along X-axis before print
  - Prevents under-extrusion at print start

### Changed
- **Input Shaper Configuration**: Updated to new resonance test results
  - X-axis: 3hump_ei shaper @ 123.0 Hz (improved from 41.667 Hz)
  - Y-axis: zv shaper @ 61.0 Hz (improved from 35.714 Hz)
  - Significantly improves print quality at higher speeds
- **Z-Offset Calibration**: Updated from 0 to -0.22mm for improved first layer adhesion
- **Bed Mesh**: Completely recalibrated bed mesh profile with latest probe readings
  - Better compensation for bed surface variations

### Technical Details
- Input shaper improvements allow for faster printing with reduced ringing/ghosting
- Axis twist compensation addresses mechanical flex in the X-axis gantry
- Z backlash compensation accounts for mechanical play in the Z-axis drive system

## [1.1.0] - 2025-10-31

### Added
- **Cartographer3D Probe System**: Complete integration of Cartographer touch and scanning probe
  - Replaces previous BLTouch configuration
  - Scan model calibration for high-speed probing
  - Touch model calibration for precise Z-offset determination
  - Temperature-compensated probe readings (reference: 25°C)
- **ADXL345 Accelerometer**: Added support for input shaping calibration
  - Mounted on Cartographer board via SPI
  - Enables automatic resonance frequency detection
- **Advanced Bed Meshing**: Enhanced 20x20 point bed mesh (upgraded from 5x5)
  - Bicubic interpolation with tension=0.2
  - High-resolution surface mapping for improved first layer quality
- **Cartographer MCU**: Dedicated microcontroller for probe operations
  - Independent processing for probe and accelerometer data
  - SPI communication for high-speed data transfer

### Changed
- **Probe Configuration**: Migrated from BLTouch to Cartographer system
  - X-offset: 36mm from nozzle center
  - Y-offset: 0mm from nozzle center
  - Verbose mode enabled for detailed diagnostics
- **Mesh Resolution**: Increased from 5x5 to 20x20 probe points
  - More accurate bed surface mapping
  - Better compensation for surface variations
- **Homing Procedure**: Updated to use Cartographer touch probe
  - More reliable and repeatable Z-homing
  - Faster homing cycles

### Technical Details
- Cartographer uses both scanning (fast surveying) and touch (precision) modes
- Scan model provides rapid surface profiling
- Touch model ensures accurate Z-offset calibration
- Temperature compensation ensures consistent results across thermal conditions

## [1.0.0] - 2025-06-14

### Added
- **Initial Release**: Complete Klipper configuration for BLV Cube CoreXY printer
- **Triple MCU Setup**: Main board, Z board, and auxiliary board configuration
  - Main MCU: Arduino Mega 2560 for X/Y motion control
  - Z Board: Arduino Mega 2560 for Z-axis control
  - Aux Board: Arduino Mega 2560 for thermal management
- **TMC2130 Stepper Drivers**: SPI-configured drivers with advanced features
  - Sensorless homing on X and Y axes
  - StealthChop and SpreadCycle modes
  - Configurable current limits and microstepping
- **CoreXY Kinematics**: Optimized for 310x310x400mm build volume
  - Maximum velocity: 300mm/s
  - Maximum acceleration: 3000mm/s²
- **Z-Tilt Adjustment**: Three-point automatic bed leveling
  - Independent Z motor control
  - Automatic bed tramming before prints
- **Bed Mesh Leveling**: 5x5 probe grid with bicubic interpolation
- **Thermal Management**:
  - E3D V6 hotend with PID control
  - Heated bed with PID control
  - Multiple temperature sensors (bed, chamber, MCU monitoring)
  - Part cooling fan with variable speed control
  - Hotend cooling fan with temperature-based activation
- **Pressure Advance**: Configured for improved print quality
  - Advance: 0.05
  - Smooth time: 0.040
- **Custom G-code Macros**:
  - `START_PRINT`: Complete print initialization sequence
  - `END_PRINT`: Safe print termination and cooldown
  - `PAUSE/RESUME/CANCEL_PRINT`: Print control for Fluidd interface
  - `LOAD_FILAMENT/UNLOAD_FILAMENT`: Filament management
- **Firmware Retraction**: Optimized retraction settings
  - Length: 0.8mm
  - Speed: 50mm/s
- **Arc Support**: G-code arc commands with 1.0mm resolution
- **Input Shaping**: Initial configuration for resonance compensation
  - X-axis: 41.667 Hz (mzv)
  - Y-axis: 35.714 Hz (mzv)

### Documentation
- **README.md**: Comprehensive setup and usage guide
  - Hardware overview
  - Installation instructions
  - Calibration procedures
  - Troubleshooting guide
- **CLAUDE.md**: AI assistant integration guide
  - Repository structure overview
  - Common operations
  - Configuration architecture

### Features
- Sensorless homing reduces mechanical complexity
- Multi-MCU architecture provides dedicated processing for different systems
- PID thermal control ensures stable temperatures
- Automatic bed leveling compensates for surface variations
- Fluidd web interface integration for remote control

## [0.1.0] - 2025-06-14

### Added
- Initial configuration upload
- Basic printer.cfg structure
- Multi-board MCU configuration

---

## Upgrade Guide

### From 1.1.0 to 1.2.0

1. Backup your current configuration
2. Update `printer.cfg` with the new version
3. **Required Calibrations**:
   - Run input shaper calibration: `SHAPER_CALIBRATE`
   - Generate skew correction profile: Follow Klipper skew correction guide
   - Calibrate axis twist: `AXIS_TWIST_COMPENSATION_CALIBRATE`
   - Re-run bed mesh calibration: `BED_MESH_CALIBRATE`
4. Adjust Z-offset if needed: The new configuration uses -0.22mm
5. Test print with skew correction enabled

### From 1.0.0 to 1.1.0

1. **Hardware Required**: Install Cartographer3D probe with ADXL345 accelerometer
2. Update MCU serial paths for the new Cartographer board
3. Remove BLTouch hardware and configuration references
4. Flash Cartographer MCU firmware
5. **Required Calibrations**:
   - Run Cartographer scan calibration: `CARTOGRAPHER_CALIBRATE`
   - Run Cartographer touch calibration
   - Generate new bed mesh: `BED_MESH_CALIBRATE`
   - Calibrate input shaper: `SHAPER_CALIBRATE`
6. Adjust physical probe offset (X=36mm, Y=0mm)

---

## Support

For issues or questions:
- Review the [README.md](README.md) for detailed configuration information
- Check [Klipper documentation](https://www.klipper3d.org/) for general Klipper help
- Consult [Cartographer3D documentation](https://docs.cartographer3d.com/) for probe-specific questions
