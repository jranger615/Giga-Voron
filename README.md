# Giga-Voron

#### 4-25-2026 - adding LOW version of my Adapter, this lowers the mount by 10mm (Raises the voron), which should allow for use of the stock connection cable. I HAVE not tested it out but it should work fine, just make sure to keep your stock stepper Z min position if you use it. 


## Things you may want to buy
* To save some assembly steps and the need for buy tons of screws you can by a fully assembled stealth burner. It will require dissassembly to attach to the new mounts and replace board/wiring
https://www.formbot3d.com/products/stealthburner-extruder-for-troodon-20?VariantsId=11006
* The stock board uses  ZH1.5 Connectors, you can buy connector packs on amazon, for ease of use, i buy the pre-crimped ones as they are tiny.
* 2510 24v Fan (Optional for Extruder Board Cooling)
* Longer Cable for the CAN  XT30 2+2. I extened mine but also made a new model of the mount to raise it up for the stock use. Will need to adjust your Z if you do this
* Some M3 screws for assembly (Various sizes for the LID, LID Fan, and Installing the board and adapter
  
## Things to Print
* Your StealthBurner and Prefered Hot End Assembly (If you dont buy and dissasemble formbot)
* The Adapter of Choice from the STL Folder (Beacon or Stock Sensor) I have no started the beacon config yet)
* Board Adapter, this allows you to install the stock board on the Voron
* LID of Choice, Standard or FAN LID. This is different then stock stealth burner to accommodate the stock board install 

## ASSEMBLY Notes
* You will have to open up the top holes on the adapter a little or just work your M3 screws through them before install on the stealth burner.
* Light and Stock sensor are very tight fit. Install them before your stealthburner
* Screws will be longer then the stock mount screws. Make sure you dont get them too long or the mount might not sit flush

## Stealthburner
Assemble the VORON Stealthburner if you dindt buy the formBot. Remember we use a different LID and Board Adapter.
* Follow the build guide located here. This is a large step, and your toolhead will only function as well as you assemble it, so take your time.
* this video is also helpful https://youtu.be/dOyYvnehLaI?si=HIRMF422sOZMZnhB
  
  
## FormBot Moons Stepper Motor Wiring Ref:
* Blue >  Blue (On Stock Connector)
* Red > Red (On Stock Connector)
* Orange > Black (On Stock Connector)
* Black > Green (On Stock Connector)

## Fans:
You can use the Stock Fans from the Giga 
* Bottom fan wire will need to be extended.
* Top Fan, you will have to split the casing apart to fit in the Voron
* Optional 2510 Fan for the Board Cooling Funtion

## Can Bus Wiring XT30 2+2
*  Can Low - Green
*  Can High - Black
*  24V - Red
*  Ground - White

# PRINTER.CFG CHANGES
Y Offset changes everywhere to +15 to accomdate the size of the VORON

## In Probe Offsets
  ```ruby
y_offset: 16
x_offset: 0
z_offset: 10.0      # ← Add +10 (or start with +9.5 and fine-tune)
 ```

## In [stepper_y]
```ruby
 position_min: 29  # Nozzle was more forward then stock and the case was 15 mm bigger. After testing 29 brings the nozzle to the front 
 ```

## In [bed_mesh]
```ruby
 mesh_min:10,45
 ```

## In [gcode_macro CANCEL_PRINT]
  ```ruby
{% set y_park = params.Y|default(printer.toolhead.axis_minimum.y + 45)|int %}   # changed +30 → +45
 ```

## In [gcode_macro PAUSE]
  ```ruby
{% set y_park = params.Y|default(printer.toolhead.axis_minimum.y + 45)|int %}   # changed +30 → +45
 ```

## In [homing_override]
  ```ruby
    {% set y_park = 218 | float %}     # was 203 → add +15
 ```

## In [stepper_z]
  ```ruby
position_min: -22      # ← Change from -12 to -22 (gives more negative room)
 ```

## In [stepper_x]
  ```ruby
position_min: -1     # ← Change from -5 to -1 Position the nozzle at edge 
 ```

## in PLR.CFG
  ```ruby
[delayed_gcode KINEMATIC_POSITION]
initial_duration:0.2
gcode:
      SET_KINEMATIC_POSITION X=0
      SET_KINEMATIC_POSITION Y=25 #Changed from 0 to make sure we dont crash into the front
      SET_KINEMATIC_POSITION Z=0
 ```

## In znp_thr1.cfg (Which ever port you use for yours)
* gear_ratio: 50:10
* rotation_distance: 22.635 #This needs to be adjusted and Calibrated to youyr machine.  Stock start point 22.68 
* sensor_type: ATC Semitec 104NT-4-R025H42G # Change to match Sensor

## Orca Slicer Changes (Specific to Bambu Labs X1C Hotend)
* In the Machine Settings, Set Extruder 1, Retraction length to 1.1
* Layer Heights Limits, Set Max to .48
