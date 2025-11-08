# BLV Cube Klipper Configuration

This repository contains the Klipper configuration for a custom BLV Cube CoreXY 3D printer.

**Current Version: v1.2.0 (2025-11-08)**

## Documentation

- **[RELEASES.md](RELEASES.md)** - User-friendly release notes with upgrade guides
- **[CHANGELOG.md](CHANGELOG.md)** - Detailed changelog following semantic versioning
- **[CLAUDE.md](CLAUDE.md)** - AI assistant integration guide

## Hardware Overview

- **Kinematics**: CoreXY
- **Control Boards**: 4x Microcontrollers
  - Main MCU: Arduino Mega 2560 - X/Y motion and primary functions
  - Z Board: Arduino Mega 2560 - Dedicated Z-axis control
  - Aux Board: Arduino Mega 2560 - Heaters and temperature management
  - Cartographer: Dedicated probe controller with ADXL345 accelerometer
- **Stepper Drivers**: TMC2130 with sensorless homing (X/Y axes)
- **Z-Axis**: Triple independent Z motors with automatic tilt adjustment
- **Bed Leveling**: 20x20 high-resolution mesh with Cartographer3D probe

## Features

- Sensorless homing on X and Y axes
- Automatic Z-tilt adjustment for bed tramming
- High-resolution bed mesh (20x20) with bicubic interpolation
- Cartographer3D touch and scanning probe
- Axis twist compensation for improved accuracy across build plate
- Z backlash compensation for precise layer positioning
- Skew correction for dimensional accuracy
- Advanced input shaping (3hump_ei @ 123Hz X, zv @ 61Hz Y)
- ADXL345 accelerometer for automatic resonance calibration
- PID-controlled bed and hotend heating
- Pressure advance calibration
- Fluidd web interface integration

## Installation

1. Install Klipper on your Raspberry Pi following the [official documentation](https://www.klipper3d.org/Installation.html)
2. Clone this repository to your home directory:
   ```bash
   git clone https://github.com/yourusername/blvcube-klipper.git ~/printer_config
   ```
3. Update the MCU serial paths in `config/printer.cfg` to match your system:
   ```bash
   ls /dev/serial/by-id/
   ```
4. Restart Klipper to apply the configuration:
   ```bash
   sudo systemctl restart klipper
   ```

## Calibration

Before first use, calibrate the following:

1. **Z-Offset**: Run `PROBE_CALIBRATE` to set the correct nozzle-to-bed distance
2. **PID Tuning**: 
   - Hotend: `PID_CALIBRATE HEATER=extruder TARGET=240`
   - Bed: `PID_CALIBRATE HEATER=heater_bed TARGET=60`
3. **Bed Mesh**: Run `BED_MESH_CALIBRATE` to create a bed mesh profile
4. **Z-Tilt**: Run `Z_TILT_ADJUST` to level the bed using three Z motors
5. **Pressure Advance**: Follow Klipper's pressure advance tuning guide

## Usage

### Basic Print Workflow

1. Heat the bed and nozzle to target temperatures
2. Home all axes: `G28`
3. Run Z-tilt adjustment: `Z_TILT_ADJUST`
4. Load bed mesh: `BED_MESH_PROFILE LOAD=default`
5. Start your print with the slicer's start G-code calling `START_PRINT`

### Useful Commands

- `START_PRINT BED_TEMP=60 EXTRUDER_TEMP=210` - Begin print sequence
- `END_PRINT` - End print and park toolhead
- `LOAD_FILAMENT` - Load filament with guided extrusion
- `UNLOAD_FILAMENT` - Unload filament with retraction
- `PAUSE` / `RESUME` - Pause and resume printing
- `CANCEL_PRINT` - Cancel current print and cool down

## Configuration Files

- `config/printer.cfg` - Main Klipper configuration file containing all printer settings

## Troubleshooting

### Sensorless Homing Issues
- Adjust `driver_SGT` values in TMC2130 sections (0-63, lower = more sensitive)
- Ensure proper motor wiring and belt tension

### Z-Tilt Problems
- Verify all three Z motors are connected to the correct pins
- Check that Z endstop is functioning properly
- Ensure bed mounting allows for slight movement

### Temperature Fluctuations
- Re-run PID calibration for the affected heater
- Check thermistor connections and wiring

## License

This configuration is provided as-is for the BLV Cube community.