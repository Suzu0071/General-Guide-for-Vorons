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
## Heaters
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

To modify the config, find the section of the stepper you want to change (e.g. [stepper_x]), and in "dir_pin", put or remove a ! before the pin number. For example, if it was "dir_pin: PC3", you would set "dir_pin: !PC3".
