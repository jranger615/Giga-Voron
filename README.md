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
 ```
## In [stepper_y]
```ruby
 position_min: 15
 ```
## In [bed_mesh]
```ruby
 mesh_min:16,25
 ```
## Extuder Board Cooler 
** Optional for Cooling FAN on at POWER UP
 This can be set almost anywhere in printer.cfg but not in one of the MACROS

  ```ruby

[gcode_macro EXTRUDER_FAN_ON]
##Turn On Extuder Board Cooler
gcode:
SET_PIN PIN=fan1 VALUE=150

[delayed_gcode TURN_ON_EXTRUDER_FAN]
initial_duration: 2.0      # Wait 2 seconds after Klipper starts
gcode:
    EXTRUDER_FAN_ON
 ```
## In [gcode_macro M106]
Removed all Fan1 functions from M106

  ```ruby
#rename_existing:M106.1
gcode:
    {% if params.P is defined %}
      {% if params.S is defined %}
        SET_PIN PIN=fan{params.P|int} VALUE={params.S|int}
      {% else %}
        SET_PIN PIN=fan{params.P|int} VALUE=255
      {% endif %}
       {% endif %} #ADDED
 ```

## In [gcode_macro PRINT_START]  
Add to top of PRINT Start Macro, this turns on the Extruder Board Cooling fan when you start a print. 
  ```ruby
SET_PIN PIN=fan1 VALUE=150
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
