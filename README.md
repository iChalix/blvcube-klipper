# BLV Cube Klipper Configuration

This repository contains the Klipper configuration for a custom BLV Cube CoreXY 3D printer.

## Hardware Overview

- **Kinematics**: CoreXY
- **Control Boards**: 3x Arduino Mega 2560 (RAMPS 1.4)
  - Main MCU: X/Y motion and primary functions
  - Z Board: Dedicated Z-axis control
  - Aux Board: Heaters and temperature management
- **Stepper Drivers**: TMC2130 with sensorless homing (X/Y axes)
- **Z-Axis**: Dual independent Z motors with automatic tilt adjustment
- **Bed Leveling**: 20x20 high-resolution mesh with Cartographer3D scanning probe
- **Accelerometer**: ADXL345 for input shaping and resonance compensation

## Features

- Sensorless homing on X and Y axes
- Cartographer3D scanning probe for high-precision bed sensing
- Automatic Z-tilt adjustment for bed tramming (2-point leveling)
- High-resolution 20x20 bed mesh with bicubic interpolation
- PID-controlled bed and hotend heating
- Pressure advance calibration
- Input shaping with ADXL345 accelerometer for resonance compensation
- Optimized TMC2130 drivers with interpolation and StealthChop
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

1. **Cartographer Probe**: Run `CARTOGRAPHER_CALIBRATE` to calibrate the scanning probe
2. **Z-Offset**: Run `PROBE_CALIBRATE` to set the correct nozzle-to-bed distance
3. **PID Tuning**:
   - Hotend: `PID_CALIBRATE HEATER=extruder TARGET=240`
   - Bed: `PID_CALIBRATE HEATER=heater_bed TARGET=60`
4. **Z-Tilt**: Run `Z_TILT_ADJUST` to level the bed using two Z motors
5. **Bed Mesh**: Run `BED_MESH_CALIBRATE` to create a high-resolution 20x20 bed mesh profile
6. **Input Shaping**: Run `SHAPER_CALIBRATE` to optimize resonance compensation with ADXL345
7. **E-Steps**: Calibrate extruder steps (rotation_distance = 3.89 is a starting point)
8. **Pressure Advance**: Follow Klipper's pressure advance tuning guide

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
- Verify both Z motors are connected to the correct pins
- Check that Cartographer probe is functioning properly
- Ensure bed mounting allows for slight movement during tilt adjustment

### Temperature Fluctuations
- Re-run PID calibration for the affected heater
- Check thermistor connections and wiring

## License

This configuration is provided as-is for the BLV Cube community.