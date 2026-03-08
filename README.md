# 🖨️ Ender 3 NG - Advanced Klipper Configuration

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Klipper](https://img.shields.io/badge/Klipper-3D%20Printer-blue.svg)](https://www.klipper3d.org/)
[![Made with Love](https://img.shields.io/badge/Made%20with-❤️-red.svg)](https://github.com/exibiliar)

A modern, feature-rich Klipper configuration for Ender 3 NG CoreXY printer with Orbiter 2.0 direct drive extruder, featuring 40+ advanced macros, intelligent LCD menu system, and comprehensive print management tools.

## ✨ Features

- 📋 **Advanced LCD Menu System** - Clean, organized ST7920 12864 display interface with no bloat
- 🎯 **40+ Smart Macros** - Automated print management, filament handling, and calibration tools
- 📊 **Print Statistics** - Track total print time, filament usage, and current print progress
- ⚡ **Quick Actions** - One-touch access to common operations (preheat, load, unload, emergency retract)
- 🎨 **Material Profiles** - Pre-configured settings for PLA, PETG, ABS, ASA, and TPU
- 🔧 **Orbiter 2.0 Optimized** - All speeds and sequences tuned for Orbiter 2.0 direct drive
- 🤖 **Smart Automation** - Auto bed mesh loading, intelligent pause/resume, automated filament change
- 📈 **Calibration Tools** - Pressure Advance pattern generator, interactive Z-offset, probe accuracy testing
- 💾 **Persistent Settings** - Temperature memory, quick preheat, material-specific PA values
- 🛡️ **Safety Features** - Temperature verification, position checks, smart Z-lift calculations

## 🎯 Perfect For

- Ender 3 NG CoreXY conversions
- Orbiter 2.0 / Orbiter 1.5 direct drive setups
- BLTouch / 3DTouch probe users
- ST7920 LCD 12864 displays
- Anyone wanting professional-grade Klipper macros
- Users who want full control from the LCD screen

## 🛠️ Hardware Compatibility

### Tested Configuration
- **Printer:** Ender 3 NG (CoreXY conversion)
- **Extruder:** Orbiter 2.0 Direct Drive
- **Probe:** BLTouch v3.1
- **Display:** ST7920 LCD 12864
- **MCU:** STM32G0B1 (via USB serial)
- **Steppers:** TMC2209 (X/Y/Z/E) in UART mode
- **Heated Bed:** 235×235mm with 6x6 mesh
- **Max Temperatures:** Hotend 300°C, Bed 100°C

### Should Work With
- Other direct drive extruders (adjust PA values)
- Different probes (CR Touch, Klicky, etc.)
- Various MCU boards (SKR, Octopus, etc.)
- Different display types (though menu is optimized for ST7920)
- TMC2208/TMC2226/TMC5160 drivers

## 📚 Software Requirements

### Klipper Installation

This configuration requires Klipper firmware. If you don't have it installed:

1. **Install Klipper** following the [official guide](https://www.klipper3d.org/Installation.html)
2. **Install Mainsail or Fluidd** for web interface
3. **Flash MCU** with Klipper firmware

### Minimum Klipper Version

- Klipper v0.11.0 or newer recommended
- Python 3.7+ on host machine

## 🚀 Installation

### 1. Clone Repository

```bash
cd ~/printer_data/config
git clone https://github.com/exibiliar/klipper-ender3ng.git
cd klipper-ender3ng
```

### 2. Backup Existing Configuration

```bash
cp ../macros.cfg ../macros.cfg.backup
cp ../menu.cfg ../menu.cfg.backup
```

### 3. Copy Files

```bash
cp macros.cfg ..
cp menu.cfg ..
```

### 4. Update printer.cfg

Add these lines to your `printer.cfg`:

```ini
[include macros.cfg]
[include menu.cfg]
```

### 5. Configure Slicer

**PrusaSlicer / SuperSlicer:**

Start G-code:
```gcode
START_PRINT BED_TEMP=[first_layer_bed_temperature] EXTRUDER_TEMP=[first_layer_temperature]
```

End G-code:
```gcode
END_PRINT
```

**Cura:**

Start G-code:
```gcode
START_PRINT BED_TEMP={material_bed_temperature_layer_0} EXTRUDER_TEMP={material_print_temperature_layer_0}
```

End G-code:
```gcode
END_PRINT
```

### 6. Restart Klipper

```bash
sudo systemctl restart klipper
```

Or via web interface:
```
FIRMWARE_RESTART
```

## 🌐 LCD Menu System

### Menu Structure

```
=== MAIN MENU ===
├─> Print Files          (SD card / virtual SD)
├─> Control              (homing, positioning, movement)
├─> Temperature          (preheats for all materials)
├─> Filament            (Orbiter 2.0 optimized operations)
├─> Macros              (advanced features)
│   ├─> Set Material    (PA presets)
│   ├─> Print Helpers   (start/end/purge)
│   ├─> Calibration     (PA/probe/Z-offset)
│   ├─> Quick Actions   (fast operations)
│   └─> Statistics      (print tracking)
├─> Klipper Tools       (PA, velocity, motors, diagnostics)
└─> Setup               (PID, calibration, restart)
```

### Quick Access Features

**From LCD you can:**
- Set material type (auto-configures Pressure Advance)
- Quick preheat to last used temperatures
- Load/unload filament with one button
- Emergency retract for blob prevention
- View print statistics
- Adjust PA, velocity, and motor current
- Run calibrations (bed mesh, Z-offset, PA)
- Check system diagnostics

## 📱 Material Profiles

### Pre-configured Materials

Pressure Advance values optimized for Orbiter 2.0 direct drive:

| Material | Nozzle Temp | Bed Temp | PA Value | Notes |
|----------|-------------|----------|----------|-------|
| **PLA**  | 220°C       | 60°C     | 0.035    | Standard settings |
| **PETG** | 245°C       | 80°C     | 0.050    | Higher PA for stringing control |
| **ABS**  | 250°C       | 100°C    | 0.040    | Enclosed printer recommended |
| **ASA**  | 260°C       | 100°C    | 0.042    | Similar to ABS, UV resistant |
| **TPU**  | 230°C       | 40°C     | 0.065    | Print slow (20-40mm/s) |

### Using Material Profiles

**Via LCD:**
1. Navigate to `Macros > Set Material`
2. Select your material (e.g., PLA)
3. PA is automatically set to optimal value

**Via G-code:**
```gcode
SET_MATERIAL MATERIAL=PLA
```

**Via Console:**
```
SET_MATERIAL MATERIAL=PETG
```

## 🎮 Essential Macros

### Print Management

#### START_PRINT
Smart print start sequence with automatic bed mesh loading and purge line.

```gcode
START_PRINT BED_TEMP=60 EXTRUDER_TEMP=220
```

**What it does:**
1. Pre-heats bed to 80% (faster warmup)
2. Homes all axes
3. Heats bed to full temperature
4. Loads saved bed mesh (if available)
5. Heats nozzle to target temperature
6. Draws purge line at front
7. Ready to print!

#### END_PRINT
Safe print end with intelligent parking and shutdown.

```gcode
END_PRINT
```

**What it does:**
1. Retracts filament (prevents oozing)
2. Calculates safe Z-lift (respects max Z)
3. Parks nozzle at rear
4. Turns off all heaters
5. Disables steppers
6. Displays "Print Complete!"

### Filament Management

#### FILAMENT_CHANGE
Automated mid-print filament change.

**Usage:** During print, run `FILAMENT_CHANGE` from console or M600

**What it does:**
1. Pauses print safely
2. Parks nozzle in accessible position
3. Unloads current filament (Orbiter sequence)
4. Waits for new filament insertion
5. User clicks RESUME when ready
6. Auto-loads new filament
7. Resumes print!

#### LOAD_FILAMENT / UNLOAD_FILAMENT
Orbiter 2.0 optimized loading sequences.

```gcode
LOAD_FILAMENT                    # Uses 230°C default
LOAD_FILAMENT EXTRUDER_TEMP=240  # Custom temperature
UNLOAD_FILAMENT                  # Uses 220°C default
```

**Optimizations:**
- Purge speed: 450mm/min (7.5mm/s)
- Load: 60mm fast + 100mm purge
- Unload: Proper cooling pause + push-back technique
- 65mm slow extraction prevents jams

### Quick Actions

#### QUICK_PREHEAT
Heats to your last used temperatures - one command!

```gcode
QUICK_PREHEAT
```

**How it works:**
1. Remembers last print temperatures
2. One button = instant preheat
3. Perfect for same material reprints

**To set custom temps:**
```gcode
# Heat to desired temps, then:
SAVE_PREHEAT_TEMPS
# Now QUICK_PREHEAT will use these!
```

#### EMERGENCY_RETRACT
Immediate 10mm retraction for blob prevention.

```gcode
EMERGENCY_RETRACT
```

**When to use:**
- Blob forming during print
- Nozzle dragging through print
- Stringing getting bad
- Quick retract needed

#### QUICK_PURGE
Fast 10mm purge for nozzle testing.

```gcode
QUICK_PURGE
```

### Calibration Tools

#### G29 (Enhanced Bed Mesh)
Auto-leveling with save and park.

```gcode
G29                # Default 60°C bed
G29 BED_TEMP=80    # Custom bed temp (for PETG)
```

**What it does:**
1. Homes all axes
2. Heats bed to specified temperature
3. Runs BED_MESH_CALIBRATE (6x6 grid)
4. Saves mesh as "default"
5. Auto-runs SAVE_CONFIG
6. Parks nozzle at center

**Important:** Mesh is loaded automatically by START_PRINT!

#### PA_CALIBRATE
Pressure Advance test pattern generator.

```gcode
PA_CALIBRATE                                    # Default range 0.0-0.1
PA_CALIBRATE PA_START=0.02 PA_END=0.08         # Custom range
PA_CALIBRATE PA_START=0 PA_END=0.1 PA_STEP=0.01  # Bigger steps
```

**How to use:**
1. Run macro - prints test pattern
2. Each line = different PA value
3. Find line with cleanest corners
4. Calculate: PA = START + (line_number × STEP)
5. Set value: `SET_PRESSURE_ADVANCE ADVANCE=0.xxx`
6. Add to printer.cfg `[extruder]` section

#### CALIBRATE_Z_OFFSET
Interactive Z-offset calibration helper.

```gcode
CALIBRATE_Z_OFFSET
```

**What it does:**
1. Homes all axes
2. Moves to bed center
3. Lowers to Z=0.2mm
4. Shows message: "Adjust Z with LCD"

**Then:**
1. Use LCD: `Tune > Offset Z`
2. Adjust until paper has slight resistance
3. Save: `Setup > Calibration > Z-Offset > Save`

### Statistics & Monitoring

#### PRINT_STATS
Shows total printing statistics.

```gcode
PRINT_STATS
```

**Displays:**
- Total print time (hours/minutes)
- Total filament used (meters/mm)
- Current printer state
- Output in console

#### CURRENT_STATS
Shows current print information.

```gcode
CURRENT_STATS
```

**Displays:**
- File name
- Progress percentage
- Print time elapsed
- Current layer (if available)

## ⚙️ Configuration & Tuning

### Adjustable Settings

#### Service Limits

In `macros.cfg`:

```python
# Material PA values
PLA:  0.035   # Adjust ±0.005 based on your setup
PETG: 0.050
ABS:  0.040
ASA:  0.042
TPU:  0.065

# Purge line settings
PURGE_LENGTH: 30mm    # Increase for more purge
PURGE_HEIGHT: 0.3mm   # First layer height
PURGE_SPEED: 1500mm/min

# Temperature defaults
LOAD_TEMP: 230°C      # Default load temperature
UNLOAD_TEMP: 220°C    # Default unload temperature
```

#### Menu Customization

In `menu.cfg`:

```python
# Preheat temperatures (change per your preference)
PREHEAT_PLA:  220/60
PREHEAT_PETG: 245/80
PREHEAT_ABS:  250/100
PREHEAT_ASA:  260/100
PREHEAT_TPU:  230/40
```

### Orbiter 2.0 Speeds

All macros use these optimized speeds:

```python
Purge Speed:     450 mm/min  (7.5 mm/s)
Fast Load:       450 mm/min
Slow Extract:    300 mm/min  (5 mm/s, for cooling)
Quick Retract:   3600 mm/min (60 mm/s)
```

### Performance Tuning

**For Faster Calibration:**
```gcode
# Reduce probe samples
[bltouch]
samples: 1              # From 2 (faster but less accurate)
```

**For Better Quality:**
```gcode
# Increase probe samples  
[bltouch]
samples: 3              # From 2 (slower but more accurate)
```

**Adjust Mesh Density:**
```gcode
[bed_mesh]
mesh_min: 0, 0
mesh_max: 183, 181
probe_count: 5, 5       # From 6,6 (faster mesh)
```

## 🐛 Troubleshooting

### Common Issues

#### "Move out of range" Error

**Cause:** Trying to move beyond printer limits
- **Solution:** Check `printer.cfg` for correct `position_max` values
- Verify bed mesh `mesh_max` doesn't exceed physical limits

```ini
[stepper_x]
position_max: 235       # Adjust to your actual bed size

[bed_mesh]
mesh_max: 183, 181      # Must be within position_max
```

#### START_PRINT Doesn't Heat Bed

**Cause:** Temperature parameters not passed from slicer
- **Solution:** Verify slicer start G-code includes temperature variables
- Test manually: `START_PRINT BED_TEMP=60 EXTRUDER_TEMP=220`

#### Filament Not Loading Properly

**Cause:** Wrong temperature or Orbiter sensor issue
- **Solution:** Check temperature is above 190°C
- Verify filament reaches Orbiter gears
- Clean Orbiter gears if clicking occurs

**For Orbiter 2.0 users:**
```gcode
# Your Orbiter2_SmartSensor.cfg already has auto-load
# Make sure it's enabled in _SENSOR_VARIABLES:
variable_disable_autoload: False
```

#### PA Calibration Pattern Looks Wrong

**Cause:** Wrong speed or temperature for material
- **Solution:** Ensure correct material temperature:
  - PLA: 220°C
  - PETG: 245°C
  - ABS: 250°C
  - TPU: 230°C (print SLOW!)

**Adjust test speeds:**
```gcode
# In PA_CALIBRATE macro, change:
G1 X100 Y{...} E10 F3000    # F3000 = print speed
# Try F1500 for TPU, F6000 for speed testing
```

#### LCD Menu Shows Chinese Characters

**Cause:** Unicode characters not supported by ST7920
- **Solution:** Already fixed in menu.cfg!
- All special characters replaced with ASCII
- If you see it, update to latest menu.cfg

#### Bed Mesh Not Loading Automatically

**Cause:** No default mesh saved, or START_PRINT not used
- **Solution:** Run `G29` once to create and save mesh
- Verify you're using START_PRINT in slicer (not manual G28/G29)

**Check if mesh exists:**
```gcode
BED_MESH_PROFILE LOAD=default
# If error: mesh doesn't exist, run G29
```

#### Emergency Retract Doesn't Work

**Cause:** Extruder temperature too low
- **Solution:** Klipper prevents extrusion below `min_extrude_temp` (190°C)
- Heat nozzle first, or lower min temp in printer.cfg:

```ini
[extruder]
min_extrude_temp: 170    # Lower for cold pulls (use carefully!)
```

### Advanced Troubleshooting

#### Enable Debug Output

Add to console/macro:
```gcode
{% set debug = True %}
```

#### Check Macro Variables

```gcode
# See all printer variables
DUMP_PARAMETERS

# Check specific values
{ action_respond_info("Free RAM: %d" % printer.system_stats.free_mem) }
{ action_respond_info("Current PA: %.3f" % printer.extruder.pressure_advance) }
```

#### Verify File Includes

In Klipper console:
```
# Should show both files loaded
CONFIG_PRINT
```

Look for:
```
[include macros.cfg]
[include menu.cfg]
```

## 📈 Performance Specifications

### Print Statistics

- **Bed Mesh Time:** ~2-3 minutes (6x6 grid, 2 samples per point)
- **START_PRINT Duration:** ~5-7 minutes (including heatup and mesh load)
- **Filament Load Time:** ~15-20 seconds (60mm + 100mm purge)
- **Filament Unload Time:** ~20-25 seconds (with cooling pause)
- **PA Calibration Pattern:** ~10-15 minutes (20 lines, 0.005 steps)
- **Emergency Retract Response:** Immediate (<1 second)

### System Resources

- **Macro File Size:** ~21KB (macros.cfg)
- **Menu File Size:** ~25KB (menu.cfg)
- **RAM Usage:** Negligible (macros run on host, not MCU)
- **EEPROM Usage:** None (all config in text files)

### Supported Ranges

- **Print Speed:** 10-400 mm/s (limited by kinematics)
- **Acceleration:** 100-20000 mm/s² (adjustable via LCD)
- **Pressure Advance:** 0-0.2 (0.035-0.065 typical for Orbiter)
- **Temperature Range:** 0-300°C hotend, 0-100°C bed
- **Motor Current:** 0.1-1.5A per motor (TMC2209)

## 🔄 Updates & Roadmap

### Current Version: 2.0

**Latest Features (v2.0):**
- ✅ 40+ advanced macros optimized for Orbiter 2.0
- ✅ Complete LCD menu system (ST7920 12864)
- ✅ Print statistics tracking
- ✅ Quick actions for common operations
- ✅ Material profiles with optimized PA
- ✅ Smart START_PRINT/END_PRINT
- ✅ Automated FILAMENT_CHANGE
- ✅ PA calibration pattern generator
- ✅ Interactive Z-offset calibration
- ✅ Enhanced PAUSE/RESUME with parking
- ✅ Quick purge and emergency retract
- ✅ Temperature memory system

### Planned Features

- [ ] Multi-color print support (macro-based, no MMU)
- [ ] Filament runout sensor integration
- [ ] Historical statistics graphs
- [ ] Nozzle wipe/purge bucket macros
- [ ] Custom sound notifications (beeper support)
- [ ] Timelapse macro integration
- [ ] Adaptive bed mesh (only print area)
- [ ] First layer calibration wizard
- [ ] Material database (save custom profiles)
- [ ] Auto Z-offset calibration (paper-less)
- [ ] Input Shaper auto-tune from LCD
- [ ] Backup/restore configuration

## 🤝 Contributing

Contributions are welcome! Here's how you can help:

### Ways to Contribute

1. **Report Bugs**
   - Found an issue? [Open an issue](https://github.com/exibiliar/klipper-ender3ng/issues)
   - Include: Klipper version, printer model, error message, steps to reproduce

2. **Suggest Features**
   - Have an idea? [Start a discussion](https://github.com/exibiliar/klipper-ender3ng/discussions)
   - Explain use case and benefits

3. **Submit Pull Requests**
   ```bash
   git checkout -b feature/YourAmazingFeature
   git commit -m 'Add some AmazingFeature'
   git push origin feature/YourAmazingFeature
   ```

4. **Improve Documentation**
   - Fix typos
   - Add examples
   - Translate to other languages

5. **Share Your Setup**
   - Post photos/videos of your build
   - Share your PA values for different filaments
   - Help others troubleshoot

### Code Style

- Use meaningful macro names (UPPERCASE_WITH_UNDERSCORES)
- Comment complex logic
- Test on real printer before submitting
- Follow existing formatting conventions

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## 👤 Author

**Exibiliar**

- GitHub: [@exibiliar](https://github.com/exibiliar)
- Project Link: [https://github.com/exibiliar/klipper-ender3ng](https://github.com/exibiliar/klipper-ender3ng)

## 🙏 Acknowledgments

- [Klipper Team](https://www.klipper3d.org/) for the amazing 3D printer firmware
- [Orbiter Team](https://www.orbiterprojects.com/) for the excellent extruder
- [Andrew Ellis](https://github.com/AndrewEllis93) for print tuning guides and inspiration
- [VoronDesign](https://vorondesign.com/) community for macro best practices
- All Ender 3 modders who pioneered CoreXY conversions
- Everyone who tested and provided feedback

## 💬 Support & Community

- **Issues:** Found a bug? [Report it](https://github.com/exibiliar/klipper-ender3ng/issues)
- **Discussions:** Questions or ideas? [Join discussion](https://github.com/exibiliar/klipper-ender3ng/discussions)
- **Pull Requests:** Improvements welcome!
- **Discord:** [Klipper Discord](https://discord.klipper3d.org/) - Find me as @exibiliar

## 📚 Useful Resources

### Official Documentation
- [Klipper Documentation](https://www.klipper3d.org/)
- [Klipper G-Code Commands](https://www.klipper3d.org/G-Codes.html)
- [Klipper Configuration Reference](https://www.klipper3d.org/Config_Reference.html)

### Tuning Guides
- [Ellis' Print Tuning Guide](https://github.com/AndrewEllis93/Print-Tuning-Guide) - Comprehensive tuning
- [Pressure Advance Tuning](https://www.klipper3d.org/Pressure_Advance.html) - Official guide
- [Resonance Compensation](https://www.klipper3d.org/Resonance_Compensation.html) - Input Shaper

### Hardware
- [Orbiter 2.0 Manual](https://www.orbiterprojects.com/orbiter-v2-0/)
- [BLTouch Installation Guide](https://www.antclabs.com/bltouch-v3)
- [TMC2209 Datasheet](https://www.trinamic.com/products/integrated-circuits/details/tmc2209-la/)

## 📜 Changelog

### v2.0.0 (2026-03-08)

**Major Update - Complete Overhaul**

#### Added
- 40+ modern macros with Orbiter 2.0 optimization
- Print statistics tracking (total time, filament used)
- Quick Actions menu (quick preheat, load, unload, emergency retract)
- Material profiles for PLA, PETG, ABS, ASA, TPU with optimized PA
- Smart START_PRINT/END_PRINT with auto bed mesh loading
- Automated FILAMENT_CHANGE workflow
- Pressure Advance calibration pattern generator
- Interactive Z-offset calibration helper
- Enhanced PAUSE/RESUME with intelligent parking
- Quick purge and emergency retract macros
- Temperature memory for quick preheat
- Complete LCD menu reorganization
- Klipper advanced tools accessible from LCD
- TMC motor current control from menu
- Velocity limits adjustment from menu

#### Changed
- Renamed "OctoPrint" menu to "Print Files" for clarity
- Optimized all filament operations for Orbiter 2.0 speeds (450mm/min)
- Updated PA values for direct drive (0.035-0.065 range)
- Improved menu organization with clear categories
- All temperatures optimized for each material
- Enhanced error handling and safety checks

#### Removed
- OctoPrint menu label (functionality preserved as "Print Files")
- Old basic filament load/unload macros
- Redundant and confusing menu items
- Unicode characters (replaced with ASCII for ST7920 compatibility)

#### Fixed
- Menu item ordering with explicit index values
- Orbiter 2.0 specific load/unload sequences with proper timing
- Z-lift calculations respect maximum Z position
- Temperature verification before extrusion
- Cooldown section positioning (always appears last)

### v1.0.0 (Previous)
- Initial release
- Basic macros and menu
- Standard Klipper configuration

---

For detailed changes, see [CHANGELOG.md](CHANGELOG.md)

## ⭐ Star History

If you find this project useful, please consider giving it a star! ⭐

It helps others discover this configuration and motivates continued development.

---

**Made with ❤️ for the Klipper Community**

*Happy Printing!* 🖨️
