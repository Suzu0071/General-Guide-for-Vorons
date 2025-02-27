DISCLAIMER (idk what else to call it):
Most of the information here is taken from Voron's [Initial Startup](https://docs.vorondesign.com/build/startup/) website. I just piled it neatly in one stack and added some of my touches.
_________________________________________________________
# Verify Electronics
But first, you'll need to modify your printer.cfg a little. Add this section:
```
[force_move]
enable_force_move: True
```
You can delete it after you finish the startup.
## Heaters pt.1
On the main screen of Mainsail/Fluidd, you should see a graph with temperatures. Make sure they're all staying at the same temperature and aren't going up or down.

If the temperatures don't match, make sure you got the sensor type correct in your config file and that nothing is shorting (aka check wire integrity). If the temperature isn't staying still, **UNPLUG THE PRINTER**, and make sure you got everything plugged in where it's supposed to be (bed goes into the ssr, etc.).
_________________________________________________________
## Steppers

<ins>**Important:**</ins> You need to find a way to emergency stop your printer before proceeding. Your web interface should have an emergency stop button, or the mroe drastic way is to just press the power off swtich (not recommended). This is needed if something goes wrong so the printer doesn't forcefully disassemble itself.

You have been warned.
_________________________________________________________
To test the motors, first turn off the printer and move the toolhead (X and Y) to the middle of the bed and >40mm (Z) high from the bed. Do it slowly, wouldn't want to backfeed the board. Now you'll need to do some math:
1. Grab the size of your build volume (e.g. 300x300x300mm).
2. Divide all of the sizes by 2 (e.g. 300/2=150).

Then paste this command to the Web Interface console:
```
SET_KINEMATIC_POSITION X=meow Y=meow Z=meow
```
Instead of the meows, paste the coordinates you got in the previous step. In my example it would be "SET_KINEMATIC_POSITION X=150 Y=150 Z=150"

Then paste:
```
G91
```
To tell the printer to use relative moves.

Now for the fun part, first moves. Paste:
```
G1 F4000 X10
```
This should move the toolhead right 10mm (if everything is setup correctly). Don't worry if it doesn't, we'll do the Y axis and then adjust everything.

To move the Y, paste this:
```
G1 F4000 Y10
```
Y axis should've moved 10mm close to the back of the printer (for corexy) or the bed should've moved out 10mm (for switchwire).

To move the Z, paste this:
```
G1 F500 Z10
```
For Trident and V0, the bed should've moved down. For V2.4 and other floating gantry, the gantry should've went up. For Switchwire, the gantry should've moved up.

You can now disable steppers:
```
M84
```

Now adjust your config and wiring based on the information of this small test:

CoreXY:

![](https://github.com/Suzu0071/General-Guide-for-Vorons/blob/main/Images/corexy-motor-configuration-guide.png)

CoreXZ:

![](https://github.com/Suzu0071/General-Guide-for-Vorons/blob/main/Images/corexz-motor-configuration-guide.png)

Using these pictures, determine which situation your printer is in, and modify the config file or the wiring.

Before plugging and unplugging motors, power down the printer.

To modify the config, find the section of the stepper you want to change (e.g. [stepper_x]), and in "dir_pin", put or remove a ! before the pin number. For example, if it was "dir_pin: PC3", you would set "dir_pin: !PC3".

Run the test again after you're done configuring.

_________________________________________________________
## Endstops
### Check Endstops
Make sure that none of the endstops are pressed.

To check endstops, we will use the QUERY_ENDSTOP command, you can run it now:
```
QUERY_ENDSTOP
```
It should return the state of all endstops, and all of them should say open.

Then, manually press the X endstop (if your printer is next to you, you can press it with your finger and enter the already pasted in command in the console. If it's not, you can move the toolhead to it and make sure that it's pressing).

It should return X as triggered. If it does the opposite, return triggered when it's not pressed, and open when it is pressed, you'll need to paste a ! before the sensor pin in the config (we learned how to do that in the stepper section c:).

if it's always open, or always triggered, make sure that you have to config right, some endstops need a pullup resistor. A pullup is ^ and configured exactly as the inverse !, just insert it before the pin number.

Repeat for Y and Z.
## First Home :D
Get your emergency stop ready, we'll be doing our first serious movement.

Home X first:
```
G28 X
```
If no problems arise, we can continue to the Y:
```
G28 X Y
```
Now that we homed X and Y, we can do the Z endstop location. This does not relate to the Switchwire, you guys can meet us at [](). V0 builders go straight to []()
### Bed Location
Before doing endstop location, you need to locate the bed first. Heres a good video about it: [Voron V2.4 Z Endstop Assembly and Bed Locating guide](https://www.youtube.com/watch?v=3hocvaTHagI). It's for V2, but should work with other models too.
### Origin Locating
To make this easier, remove the magnetic bed, leaving only the aluminum plate. This will help see the true corner of the plate. Make sure that the nozzle isn't touching the plate.

Now, we need to define the 0, 0 point for X and Y axis. First, home X and Y:
```
G28 X Y
```
Now, move the toolhead to the left-most corner where the software thinks it's at 0, 0:
```
G90
G1 X0 Y0 F6000
```
+ If the nozzle is 3-5mm away from the corner, you'll need to move the aluminum plate itself, be it moving the plate itself, or the extrusions its standing on. Make sure that when you're doing this, the Z endstop is still available to the nozzle. Run the test again if you did that.
+ If the nozzle is ~1mm away from the sides of the bed, AND if the nozzle is over the bed, nothing needs to be changed.
+ If the nozzle is ~1-2mm away from the sizes of the bed, OR if it's not over the bed, you'll need to adjust the max position in your config. (Only works when its 1-2mm away)
  + For example, if the nozzle is 2mm away from the plate, and not over it, you'll go to [stepper_x/y] section and subtract 2 from the position_max (also don't forget to set the endstop_position to the same number).
  + If the nozzle is 2mm over the plate, you'd add 2 to position_max.
 
You can now put the magnetic bed back on.
### Z Endstop Location
***For V2, Trident, V1, Legacy:***

Home X and Y:
```
G28 X Y
```
Using the movement controls in your web interface, position the nozzle straight over the Z endstop pin, and run:
```
M114
```
You will be given your homing coordinates, you will need to put them into [safe_z_home] or [homing_override] section. *Don't forget to do firmware restart.*

Now you can home every axis, but have the emergency stop ready the first time. <sub>Press the home button in the controls of your web interface.</sub>

***For V0:***

Since the V0 uses a switch to test the bed assmebly itself, the easiest way to calibrate it is to run Z_ENDSTOP_CALIBRATE.

Use the bed leveling screws and place the bed roughly in the middle of their travel. Now you'll need to run:
```
Z_ENDSTOP_CALIBRATE
```
If you're using Mainsail, a dialog box with controls will open, position the bed to the nozzle using the paper test (recommended).

If you're NOT using Mainsail, you'll need to issue manual commands:
```
TESTZ Z=meow
```
Negative numbers will move the bed closer to the nozzle, positive will move the bed further away.

When you are done, either press ACCEPT (in Mainsail), or just type ACCEPT in the console. To save the offset, type:
```
SAVE_CONFIG
```
The offset will be saved to the bottom of your config file.
_________________________________________________________
## Heaters pt.2
Now we'll be testing the heaters. Make sure you have everything set correctly in the config (e.g. max bed power).
### Hotend
On the main screen of your web interface, use the same graph we used earlier to set temperatures for the hotend. It should be named "Extruder". Set the temperature to 60.

When it passes 55, check that the hotend fan is spinning and nothing smells burnt c: You can then turn off the heater, either set the temperature to 0 or press the "Cooldown" button.
### Bed
Now do the same things for the bed heater, set the temperature to 55 and check that everything works and doesn't go up in flames. With the config from Voron github, the controller fan should start spinning when the bed is on.
_________________________________________________________
### PID tune
Now we'll tune PID for hotend and bed.

In the web interface console enter:
```
PID_CALIBRATE HEATER=heater_bed TARGET=100
```
This will make the bed heater go up to 100 and fluctuate about 5 degrees each way. When it finishes, it should spit out the PID settings in the console, copy them to a notepad and change them parameters in the [heater_bed] section.

Now the hotend:
```
M106 S64
PID_CALIBRATE HEATER=extruder TARGET=245
```
This will set the part cooling fan to 25% and run PID calibrate. Complete the same procedure as for the bed heater, except now modify the [extruder] section.
_________________________________________________________
## Bed Leveling
### V0
V0 doesn't have a probe so the bed leving is done by hand using the leveling knobs. Klipper has an inbuilt macro for helping with leveing:
```
BED_SCREWS_ADJUST
```
It will move the nozzle to each screw location and you should adjust the bed. If you turned the knob over 1/8 of a revolution, you should press "ADJUSTED", this means the nozzle will move back to this screw to recheck. If you're satisfied with the leveling on a screw, press "ACCEPT". Both buttons will still proceed to the next screw.

You can also rerun the Z_ENDSTOP_CALIBRATE command to reset the endstop position to 0.
### V1 & Legacy
First, run a macro that will do the tilt for the bed:
```
BED_TILT
```
Then:
```
BED_SCREWS_ADJUST
```
This will bring the nozzle to each screw position. See more about adjusting screws in the V0 section.
### Trident
Trident uses a fully automatic macro:
```
Z_TILT_ADJUST
```
You shouldn't need to do anything, it will probe the specified locations and move the bed accordingly.
### V2
V2 also uses a fully automatic macro:
```
QUAD_GANTRY_LEVEL
```
The printer will probe specified points and automatically adjust the gantry.
### 
