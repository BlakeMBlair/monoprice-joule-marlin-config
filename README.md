# Monoprice Joule Custom Marlin Firmware

Custom Marlin setup and pin definitions for an HC32-based Monoprice Joule 3D printer. This repository isolates all hardware-specific parameters, probe profiles, and pin reassignments to `Configuration.h` and the motherboard pin file, keeping core firmware files completely untouched.

## Demonstration


https://github.com/user-attachments/assets/3a828dcb-8d71-4dea-931d-c6dbd98641e4



## Supported Upgrades & Modifications
* **Auto Bed Leveling:** Creality CR Touch ABL sensor (`SERVO0_PIN` & `Z_MIN_PROBE_PIN`)
* **Extrusion:** Capricorn PTFE Bowden tubing
* **Cooling:** Noctua silent fans integrated via step-down buck converters
* **Print Surface:** Adjusted offset coordinates and thermal profiles for a Yoopai CoolPlay UltraTack build plate

## Wiring & Hardware Pinouts
Below are the wiring pinouts and buck converter step-down connections on the HC32 mainboard. 

<p float="left">
  <img src="media/Informaiton%20pins.jpg" width="48%" alt="HC32 Information Pins Wiring" />
  <img src="media/Power%20pins.jpg" width="48%" alt="Power & Buck Converter Wiring" />
</p>

*Note: The file paths above use `%20` to account for the spaces in your file names.*

## Modified Files
All custom configuration logic is maintained within the `/config` folder:

* `config/Configuration.h`: Handles primary baud rates, thermistor profiles, physical bounds, nozzle-to-probe offsets (`NOZZLE_TO_PROBE_OFFSET`), and bilinear bed leveling (`AUTO_BED_LEVELING_BILINEAR`).
* `config/pins_YOUR_HC32_BOARD.h`: Hardware pin mapping for the HC32 microcontroller, CR Touch control/sensor lines, and buck converter fan connections.

*Core system files (`serial.h`) and `Configuration_adv.h` remain completely stock.*

## Installation & Flashing Instructions
1. Download or clone the stock Marlin source code (`v2.1.x`).
2. Copy `Configuration.h` and `pins_YOUR_HC32_BOARD.h` from `/config` into their corresponding paths in your Marlin source tree (`Marlin/` and `Marlin/src/pins/`).
3. Compile using VSCode with PlatformIO / AutoBuild Marlin targeting your HC32 board environment.
4. Flash the compiled firmware binary (`.bin`) to the Monoprice Joule mainboard via microSD card.

## Post-Flashing Calibration
1. Execute PID autotuning for both the hotend and heated bed (`M303`).
2. Home all axes (`G28`).
3. Calibrate the CR Touch Z-offset (`M851 Z-...`) relative to the Yoopai UltraTack build plate.
4. Run an auto bed leveling probe sequence (`G29`) and save the mesh data to EEPROM (`M500`).

---
**Author:** Blake Mitchell
