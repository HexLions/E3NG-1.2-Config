# Changelog

All notable changes to this Klipper configuration will be documented in this file.

## [2.0.0] - 2026-03-08

### 🎉 Major Update - Complete Menu & Macro Overhaul

This release completely modernizes the Klipper menu and macro system with Orbiter 2.0 optimization, advanced features, and quality-of-life improvements.

---

### ✨ Added

#### Menu System
- **Print Files menu** - Clearer naming (replaced "OctoPrint")
- **Quick Actions submenu** - Fast access to common operations
  - Quick Preheat (remembers last temps)
  - Quick Load/Unload filament
  - Emergency Retract
  - Quick Purge
  - Save Preheat Temps
- **Statistics submenu** - Print tracking and monitoring
  - Total Stats (time, filament)
  - Current Print info
  - Last Print info
- **Material Profiles submenu** - 5 materials with optimized PA
  - PLA (PA=0.035)
  - PETG (PA=0.050)
  - ABS (PA=0.040)
  - ASA (PA=0.042)
  - TPU (PA=0.065)
- **Klipper Tools** accessible from LCD
  - Pressure Advance control
  - Velocity Limits adjustment
  - Motor Current (TMC)
  - Diagnostics tools

#### Macros (40+ total)

**Print Management:**
- `START_PRINT` - Smart print start with:
  - Auto bed mesh loading
  - Intelligent preheating (80% bed first)
  - Automatic purge line
  - Parameter support (BED_TEMP, EXTRUDER_TEMP)
- `END_PRINT` - Safe print end with:
  - Smart Z-lift (respects max Z)
  - Safe parking position
  - Automatic heater shutdown
  - Stepper disable
- `PURGE_LINE` - Consistent front purge line

**Pause/Resume:**
- `PAUSE` (enhanced) - Intelligent pause with:
  - Smart Z-lift calculation
  - Safe park position (rear right)
  - Automatic retraction
  - 10-minute heater timeout
  - Position save for resume
- `RESUME` (enhanced) - Smart resume with:
  - Double prime to prevent underextrusion
  - Position restoration
  - Heater reactivation
- `CANCEL_PRINT` (enhanced) - Clean cancellation with:
  - Immediate heater shutdown
  - Safe Z-lift and park
  - Bed mesh clear
  - Complete cleanup

**Filament Management:**
- `FILAMENT_CHANGE` - Automated filament change
  - Pauses print safely
  - Unloads current filament
  - Moves to accessible position
  - Waits for user to load new filament
- `LOAD_FILAMENT` - Orbiter 2.0 optimized loading
  - Temperature check/heating
  - 60mm fast load
  - 100mm purge at 450mm/min
  - Customizable temps
- `UNLOAD_FILAMENT` - Orbiter 2.0 optimized unloading
  - Temperature check/heating
  - Proper retract sequence
  - 2-second cooling pause
  - Push-back technique
  - 65mm slow extraction

**Quick Actions:**
- `QUICK_PREHEAT` - Preheat to last used temperatures
- `SAVE_PREHEAT_TEMPS` - Save current temps for quick preheat
- `QUICK_LOAD` - Fast load at saved temperature
- `QUICK_UNLOAD` - Fast unload at saved temperature
- `EMERGENCY_RETRACT` - Immediate 10mm retraction
- `QUICK_PURGE` - Fast 10mm purge

**Material Presets:**
- `PREHEAT_PLA` - 220°C / 60°C
- `PREHEAT_PETG` - 245°C / 80°C
- `PREHEAT_ABS` - 250°C / 100°C
- `PREHEAT_ASA` - 260°C / 100°C (NEW!)
- `PREHEAT_TPU` - 230°C / 40°C (NEW!)
- `SET_MATERIAL` - Sets PA for material type

**Calibration:**
- `G29` (enhanced) - Bed mesh with:
  - Auto bed heating
  - Mesh save as "default"
  - Auto SAVE_CONFIG
  - Safe parking
- `CALIBRATE_Z_OFFSET` - Interactive Z-offset calibration
- `PA_CALIBRATE` - Pressure Advance test pattern
  - Customizable PA range
  - Adjustable step size
  - Orbiter speeds (450mm/min)
- `PROBE_ACCURACY_TEST` - 10-sample probe test

**Statistics:**
- `PRINT_STATS` - Total statistics
- `CURRENT_STATS` - Current print info
- `LAST_PRINT_INFO` - Last print details

**Utilities:**
- `PARK_NOZZLE` - Park for maintenance (Z=100mm)
- `CENTER_NOZZLE` - Move to bed center (Z=50mm)
- `CLEAN_NOZZLE` - Automated wipe routine
- `COOLDOWN` - Turn off all heaters and fan
- `MOTORS_OFF` - Disable all steppers
- `PID_TUNE_HOTEND` - Auto PID tune with save
- `PID_TUNE_BED` - Auto PID tune with save

---

### 🔄 Changed

#### Performance
- All filament operations optimized for Orbiter 2.0:
  - Purge speed: 450 mm/min (7.5 mm/s)
  - Load distance: 60mm + 100mm purge
  - Unload distance: 65mm (config value)
  - Proper cooling pauses
- PA values updated for direct drive
  - Previous: 0.04-0.07
  - New: 0.035-0.065
  - Tested and optimized per material

#### Menu Organization
- "OctoPrint" → "Print Files" (clearer naming)
- Reorganized menu structure with logical grouping
- Added explicit index values for consistent ordering
- Preheat profiles now use macros (cleaner code)
- Temperature menu includes all 5 materials

#### Code Quality
- Comprehensive comments and documentation
- Consistent formatting and naming
- Better error handling
- Temperature safety checks
- Extruder capability verification

---

### 🗑️ Removed

#### Menu Items
- OctoPrint label (functionality preserved)
- Old separate preheat submenus
- Redundant filament load/unload options
- Confusing menu entries

#### Macros
- Basic old load/unload macros
- Duplicate functionality
- Unsafe operations

---

### 🐛 Fixed

#### Display Issues
- Unicode characters → ASCII (ST7920 compatibility)
  - Removed: ►, ◘, ⚙, ║, ═, ❄, ✕, ✓, °
  - Replaced with: >, text descriptions
- Menu item ordering with explicit indices
- Cooldown section positioning (always last)

#### Functionality
- Orbiter 2.0 load/unload sequences
  - Proper timing and pauses
  - Correct distances
  - Temperature verification
- PA calibration pattern speeds
- Bed mesh auto-save reliability
- Z-offset save process

---

### 📊 Statistics

- **Total Macros**: 40+
- **Menu Items**: 80+
- **Supported Materials**: 5 (PLA, PETG, ABS, ASA, TPU)
- **Lines of Code**: ~1200 (macros.cfg + menu.cfg)
- **File Size**: 46KB total

---

### 🔧 Technical Details

#### Orbiter 2.0 Settings
```ini
Purge Speed: 450 mm/min
Load Temp: 230°C (default)
Unload Temp: 220°C (default)
Unload Distance: 65mm
Purge Length: 100mm
```

#### Pressure Advance Values
```ini
PLA:  0.035
PETG: 0.050
ABS:  0.040
ASA:  0.042
TPU:  0.065
```

---

### 📝 Migration Guide

#### From v1.x to v2.0

1. **Backup existing config:**
   ```bash
   cp macros.cfg macros.cfg.v1.backup
   cp menu.cfg menu.cfg.v1.backup
   ```

2. **Update slicer start G-code:**
   ```gcode
   # Old
   G28
   G29
   M104 S[temp]
   ...
   
   # New
   START_PRINT BED_TEMP=[bed_temp] EXTRUDER_TEMP=[extruder_temp]
   ```

3. **Update slicer end G-code:**
   ```gcode
   # Old
   G91
   G1 E-2 F2700
   ...
   
   # New
   END_PRINT
   ```

4. **Remove old includes** (if any):
   - Remove old menu includes
   - Ensure only new macros.cfg and menu.cfg are included

5. **FIRMWARE_RESTART**

---

### ⚠️ Breaking Changes

- **Slicer G-code must be updated** to use START_PRINT/END_PRINT
- **Old PAUSE/RESUME behavior changed** (now parks in different position)
- **Filament load/unload speeds changed** (optimized for Orbiter 2.0)
- **Menu structure completely reorganized**

---

### 🎯 Known Issues

None currently reported.

---

### 🙏 Credits

- Klipper team for the amazing firmware
- Orbiter team for the Orbiter 2.0 extruder
- Andrew Ellis for print tuning guides
- Community feedback and testing

---

## [1.0.0] - Previous Version

Initial release with basic menu and macros.

---

For full documentation, see [README.md](README.md)
