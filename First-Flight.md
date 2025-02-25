DISCLAIMER (idk what else to call it):
Most of the information here is taken from Voron's [Initial Startup](https://docs.vorondesign.com/build/startup/) website. I just piled it neatly in one stack and added some of my touches.
_________________________________________________________
# Verify Electronics
## Heaters
On the main screen of Mainsail/Fluidd, you should see a graph with temperatures. Make sure they're all staying at the same temperature and aren't going up or down.

If the temperatures don't match, make sure you got the sensor type correct in your config file and that nothing is shorting (aka check wire integrity). If the temperature isn't staying still, **UNPLUG THE PRINTER**, and make sure you got everything plugged in where it's supposed to be (bed goes into the ssr, etc.).
_________________________________________________________
## Steppers
To test the motors, first turn off the printer and move the toolhead to the middle of the bed, >10mm high from the bed. Now you'll need to do some math:
1. Grab the size of your build volume (e.g. 300x300x300mm).
2. Divide all of the sizes by 2 (e.g. 300/2=150).
   
