# FlashForge Dreamer Cura Definitions (Dual Extruder)

## **_DISCLAIMER_**
All information is provided "as is", with no guarantee of
completeness, accuracy, timeliness or of the results obtained from the use of this information, and without warranty of any kind, express or implied, including, but not limited to warranties of performance, merchantability and
fitness for a particular purpose. I will not be liable to you or anyone else for any decision made or action taken in reliance on the information given for any consequential, special or similar damages, even if advised of the possibility of such damages.

---

**_WARNING_** - This profile adds support for the Dual Extruder Dreamer 3D printer flashed to [Marlin](https://github.com/moonglow/FlashForge_Marlin) firmware. This profile does **NOT** work with FlashForge factory firmware due to differences in the G-Code! **DO NOT** attempt to use this profile on factory firmware without changing the G-Code first!

I have tested this profile with [Cura 5.2.1](https://ultimaker.com/software/ultimaker-cura) with good results, but it may not work 100% correctly. Please submit an [issue](https://github.com/tracedgod/FlashForge-Dreamer-Cura-Definitions/issues) if you run into any problems, or if you have a change suggestion, please submit a [pull request](https://github.com/tracedgod/FlashForge-Dreamer-Cura-Definitions/pulls).

## **_Installation Instructions_**
Please follow the below instructions to install this profile to Cura:

1. Download the latest release .zip from [here](https://github.com/tracedgod/FlashForge-Dreamer-Cura-Definitions/releases/latest).
2. Unzip the files into your Cura install directory:
    - Windows: ``"C:\Program Files\Ultimaker Cura {Version}\share\cura\resources"``
    - MacOS: ``/Users/{Username}/Library/Application Support/cura/{Version}/``
    - Linux: ``~/.local/share/cura/{Version}``
3. After unzipping, open Cura and select the printer under ``FlashForge > Dreamer``

## **_G-Code (Marlin)_**
The G-Code between FlashForge firmware and Marlin firmware are quite different, and incompatible with one another. To fix this for Marlin firmware, the profile uses the below Start & End G-Codes:

### **_Start G-Code_**
```
;Start Gcode
G90
M141 S{build_volume_temperature}     ;Set chamber temperature for cooling fans
M140 S{material_bed_temperature_layer_0}     ;Heat bed up to first layer temperature
M190 S{material_bed_temperature_layer_0}     ;Wait for Heat Bed temperature
M104 S{material_print_temperature_layer_0}    ;Set nozzle temperature to first layer temperature
M109 S{material_print_temperature_layer_0}    ;Wait for Extruder temperature
M107 ;start with the fan off
G28
M117 Purge extruder
G1 Z50.000 F420
;Purge line
G1 X-110.00 Y-60.00 F4800
G1 Z{layer_height_0} F420
G92 E0
G1 X-110.00 Y60.00 E17,4 F1200
;Purge line end
G92 E0
G1 Z2.0 F3000 ; Move Z Axis up little to prevent scratching of Heat Bed
```

### **_End G-Code_**
```
;end gcode
M140 S0 ; turn off heatbed
M104 S0 ; turn off temperature
M107 ; turn off fan
M141 S0 ; turn off chamber temperature
G28 X Y
M84 X Y E ; disable motors
```

## **_G-Code (FlashForge)_**
 _The Factory G-Code support is a WIP. Eventually I plan to have separate releases for Marlin & FlashForge firmware profiles._
 
 I have **NOT** tested these G-Codes with Cura, **USE AT YOUR OWN RISK!**

 ### **_Start G-Code_**

 ```
M118 X17.70 Y17.70 Z34.89 T0
M107
G90
G28
M132 X Y Z A B
G1 Z50.000 F420
G161 X Y F3300
M7 T0
M6 T0
M651
M907 X100 Y100 Z40 A100 B20
 ```

 ### **_End G-Code_**
 ```
M104 S0 T0
M140 S0 T0
G162 Z
G28 X Y
M132 X Y A B
M652
G91
M18
 ```