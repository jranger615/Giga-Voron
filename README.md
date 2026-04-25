# Giga-Voron

## Things you may want to buy
* To save some assembly steps and the need for buy tons of screws you can by a fully assembled stealth burner. It will require dissassembly to attach to the new mounts and replace board/wiring
https://www.formbot3d.com/products/stealthburner-extruder-for-troodon-20?VariantsId=11006

* The stock board uses  ZH1.5 Connectors, you can buy connector packs on amazon, for ease of use, i buy the pre-crimped ones as they are tiny.

* 2510 24v Fan (Optional for Extruder Board Cooling)

## FormBot Moons Stepper Motor Wiring Ref:
If you cant Print ABS or Dont want to print your StealthBurner - [https://www.etsy.com](https://www.etsy.com/search?q=voron+stealthburner+clockwork+2&ref=auto-1

* Blue >  Blue (On Stock Connector)
* Red > Red (On Stock Connector)
* Orange > Black (On Stock Connector)
* Black > Green (On Stock Connector)

## Fans:
You can use the Stock Fans from the Giga 
* Bottom fan wire will need to be extended.
* Top Fan, you will have to slit the casing apart to fit in the Voron
* Optional 2510 Fan for the Board Cooling Funtion

## Can Bus Wiring XT30 2+2
*  Can Low - Green
*  Can High Black
*  24V Red
*  Ground White

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
 position_min: 15
 ```

## In [bed_mesh]
```ruby
 mesh_min:16,25
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

## in PLR.CFG
  ```ruby
[delayed_gcode KINEMATIC_POSITION]
initial_duration:0.2
gcode:
      SET_KINEMATIC_POSITION X=0
      SET_KINEMATIC_POSITION Y=25 #Changed from 0 to make sure we dont crash into the front
      SET_KINEMATIC_POSITION Z=0
 ```

## Still to change
* gear_ratio: 50:10
* rotation_distance: 22.635 #This needs to be adjusted and Calibrated to youyr machine. 
* gear_ratio: 50:10
* sensor_type: ATC Semitec 104NT-4-R025H42G # Change to match Sensor 
