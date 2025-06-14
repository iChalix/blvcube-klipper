# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This is a Klipper 3D printer configuration repository for a custom CoreXY printer with multiple microcontrollers. The printer uses three Arduino Mega 2560 boards:
- Main MCU: Controls X/Y movement and primary functions
- Z board: Dedicated controller for Z axis movement
- Aux board: Manages heaters and temperature control

## Key Configuration

The main configuration file is `config/printer.cfg` which contains:
- TMC2130 stepper drivers with sensorless homing for X and Y axes
- Dual Z-axis setup with Z-tilt adjustment
- Bed mesh leveling configuration
- Multiple temperature sensors and cooling fans
- Custom G-code macros for print operations

## Common Commands

Since this is a configuration repository, there are no build, test, or lint commands. Common operations include:

### Klipper Service Management
```bash
sudo systemctl restart klipper  # Apply configuration changes
sudo systemctl status klipper   # Check if Klipper is running
```

### Configuration Validation
```bash
~/klippy-env/bin/python ~/klipper/scripts/check_config.py config/printer.cfg
```

## Architecture

The configuration is structured around:

1. **MCU Definitions**: Three separate microcontrollers communicate via USB serial
2. **Motion System**: CoreXY kinematics with sensorless homing on X/Y
3. **Thermal Management**: Multiple thermistors, heaters, and fans with PID control
4. **User Macros**: Print start/end sequences, filament management, and Fluidd web interface integration

## Important G-code Macros

- `START_PRINT`: Initializes printer state, homes axes, heats bed/nozzle
- `END_PRINT`: Safely ends print, parks toolhead, cools down
- `LOAD_FILAMENT` / `UNLOAD_FILAMENT`: Filament change procedures
- `PAUSE` / `RESUME` / `CANCEL_PRINT`: Print control macros for Fluidd interface

## Configuration Notes

- All MCU serial paths use `/dev/serial/by-id/` for stable USB device identification
- Sensorless homing sensitivity is configured via `driver_SGT` parameter
- Z-tilt uses three independent Z motors for automatic bed tramming
- Bed mesh is configured for a 5x5 probe grid with bicubic interpolation