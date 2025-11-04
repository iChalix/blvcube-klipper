# Changelog

All notable changes to the BLV Cube Klipper configuration will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.0.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.1.0] - 2025-10-31

### Added - Cartographer3D Integration

#### Hardware Changes
- **Cartographer3D Probe System**: Integrated Cartographer3D scanning probe replacing BLTouch
  - New MCU connection via USB serial: `usb-Cartographer_614e_35002C001943303854303720-if00`
  - Added cartographer temperature monitoring sensor
  - Probe offset: X=36mm, Y=0mm from nozzle

- **ADXL345 Accelerometer**: Added accelerometer support for advanced input shaping
  - Connected via Cartographer board SPI interface
  - Enables resonance compensation tuning

#### Probing & Leveling
- **Enhanced Bed Mesh**: Upgraded from 5x5 to 20x20 probe grid
  - Much higher resolution bed mesh for improved first layer quality
  - Zero reference position at bed center (155, 155)
  - Mesh area: 40x40mm to 270x270mm
  - Adaptive margin: 10mm
  - Interpolation disabled (mesh density is sufficient)
  - Increased probing speed to 300mm/s

- **Z-Tilt Configuration**: Simplified from 3-point to 2-point leveling
  - Z positions: (0, 155) and (310, 155)
  - Probe points: (10, 155) and (260, 155)
  - Improved reliability and speed

- **Z-Homing**: Changed to probe-based virtual endstop
  - Uses Cartographer probe for Z-axis homing
  - Removed physical Z endstop requirement
  - Homing retract distance set to 0 for faster homing

#### Stepper Motor Improvements
- **X/Y Stepper TMC2130 Updates** (config/printer.cfg:65, 90):
  - Increased run current: 0.7A → 0.9A
  - Added hold current: 0.5A
  - Enabled interpolation (256 microsteps)
  - Improved motion smoothness and holding torque

- **Extruder TMC2130 Enhancements** (config/printer.cfg:170-183):
  - Increased run current: 0.5A → 0.9A
  - Added hold current: 0.5A
  - Enabled StallGuard (SGT=3) for potential filament detection
  - Optimized chopper configuration for 1.7A motors:
    - TBL=2, TOFF=3, HSTRT=0, HEND=7
  - Enabled interpolation for smoother extrusion
  - StealthChop disabled for better torque

- **Z-Axis TMC2130 Adjustments**:
  - Reduced run current: 0.85A → 0.7A (sufficient for lead screws)
  - Swapped stepper_z and stepper_z1 pin assignments for better cable routing

#### Extruder Configuration
- **Rotation Distance**: Changed from 7.78 to 3.89
  - Indicates gear ratio or extruder hardware change
  - **Important**: Verify E-steps calibration after update
- **Minimum Extrude Temperature**: Lowered from 180°C to 170°C
  - Allows cold pulls and maintenance at lower temperatures

#### Other Improvements
- **Virtual SD Card Path**: Updated to Fluidd standard location
  - Old: `~/gcode_files`
  - New: `/home/pi/printer_data/gcodes`

- **Safe Z-Home Override**: Modified homing sequence
  - Homes X/Y first
  - Moves to position (10, 10) before Z-homing
  - Prevents bed collision during homing

### Changed

- Disabled old BLTouch probe configuration (commented out)
- Updated Z-axis endstop configuration to use probe
- Modified homing override macro to include positioning move

### Calibration Required After Update

After upgrading to this release, you **must** recalibrate:

1. **Cartographer Probe**:
   ```
   CARTOGRAPHER_CALIBRATE
   ```

2. **Z-Offset**:
   ```
   PROBE_CALIBRATE
   ```

3. **Bed Mesh** (take advantage of 20x20 resolution):
   ```
   BED_MESH_CALIBRATE
   ```

4. **Z-Tilt** (new 2-point configuration):
   ```
   Z_TILT_ADJUST
   ```

5. **E-Steps** (rotation_distance changed):
   ```
   # Extrude 100mm filament and measure actual extrusion
   # Adjust rotation_distance accordingly
   ```

6. **Input Shaping** (with new ADXL345):
   ```
   SHAPER_CALIBRATE
   ```

### Technical Notes

- The Cartographer3D provides scanning probe capabilities with higher precision than mechanical probes
- The scan model coefficients have been pre-calibrated (see SAVE_CONFIG section)
- Touch model threshold: 3110, speed: 2mm/s, offset: -0.05mm
- Default bed mesh with 400 probe points is stored in configuration

### Migration from BLTouch

If you're upgrading from a BLTouch setup:
1. Update MCU serial path for Cartographer in printer.cfg
2. Remove BLTouch wiring and install Cartographer probe
3. Update probe X/Y offsets (Cartographer: X=36, Y=0)
4. Run all calibration procedures listed above
5. Test Z-homing behavior carefully before first print

---

## [1.0.0] - 2025-10-XX

### Added
- Initial BLV Cube Klipper configuration
- CoreXY kinematics with three Arduino Mega 2560 boards
- TMC2130 stepper drivers with sensorless homing (X/Y)
- Triple Z-axis with automatic tilt adjustment
- BLTouch probe for bed leveling
- Basic bed mesh (5x5 grid)
- PID-controlled heating for bed and hotend
- Fluidd web interface integration
- Print management macros (START_PRINT, END_PRINT, PAUSE, RESUME, CANCEL_PRINT)
- Filament management (LOAD_FILAMENT, UNLOAD_FILAMENT)
- Input shaping configuration (MZV shaper type)
