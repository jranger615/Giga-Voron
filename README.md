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
* Top Fan, you will have to slit the casing apart to fit in the giga
* Optional 2510 Fan for the Board Cooling Funtion

## Can Bus Wiring XT30 2+2
*  Can Low - Green
*  Can High Black
*  24V Red
*  Ground White

# PRINTER.CFG CHANGES

## Probe Offsets
  ```ruby
y_offset: 16
x_offset: 0
 ```
## [stepper_y]
```ruby
 position_min: 15
 ```
## [bed_mesh]
```ruby
 mesh_min:16,25
 ```
## Extuder Board Cooler
 This can be set almost anywhere in printer.cfg but not in one of the MACROS

  ```ruby
SET_PIN PIN=fan1 VALUE=150
 ```
## [gcode_macro M106]
Add the # marks in the 2nd part of this code or delete those sections. That removes the cooling fan on/off for them. 

  ```ruby
#rename_existing:M106.1
gcode:
    {% if params.P is defined %}
      {% if params.S is defined %}
        SET_PIN PIN=fan{params.P|int} VALUE={params.S|int}
      {% else %}
        SET_PIN PIN=fan{params.P|int} VALUE=255
      {% endif %}
    #{% else %}
    #  {% if params.S is defined %}
    #    SET_PIN PIN=fan1 VALUE={params.S|int}
    #  {% else %}
    #    SET_PIN PIN=fan1 VALUE=255 
    #  {% endif %}
    # {% endif %}
 ```

## [gcode_macro PRINT_START]  
Added incase you run multiple prints. I Macro 107 to turn off both cooling fan and the extruder board fan so a new print will start the board fan back up
  ```ruby
SET_PIN PIN=fan1 VALUE=150
 ```
