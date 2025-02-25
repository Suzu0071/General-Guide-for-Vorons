# Board Flashing and MCU IDs
If you have multiple boards, its advised to unplug every but one, and complete this for every board separately.
_________________________________________________________
## Katapult
[Katapult](https://github.com/Arksine/katapult) is a utility for flashing board more easily + doing CAN bus. You're here since you wont be using a CAN board, so we won't go into that and proceed with normal board flashing c:

### Install Katapult
```
cd ~ && git clone https://github.com/Arksine/katapult
```
Now that Katapult is installed, run:
```
cd katapult
make menuconfig
```
*Common boards are in the the Katapult folder in Board Builds*

Press q to exit and choose to save. 

Then:
```
make clean
make
```
It'll take a minute to make the file.

Heres how it should look for STM32 boards:

![](https://github.com/Suzu0071/General-Guide-for-Vorons/blob/main/Images/Katapult-Bin-Example.png)

Heres RP2040 based boards (e.g. SKR pico):

![](https://github.com/Suzu0071/General-Guide-for-Vorons/blob/main/Images/RP2040-based-Katapult-Bin.png)
_________________________________________________________
### Flash Katapult
First, enter DFU mode:

**For STM32 boards:**

Read the instructions for your specific board, but most board will have a BOOT button/jumper and a RESET button. Hold down BOOT (or insert a jumper on it) and press RESET.

**For RP2040 boards:**

Read the instructions for your specific board, but the process is the same as for STM32 boards, short the BOOT and press RESET. 

Heres an example for SKR pico:

![](https://github.com/Suzu0071/General-Guide-for-Vorons/blob/main/Images/SKR-pico-DFU.png)

Now check that the board went into DFU by running:
```
lsusb
```

STM32  boards will show up as "STM device in dfu", RP2040 will show up as "Pi RP2 Boot".
_________________________________________________________
<ins>If the board doesn't show up, check that you have everything plugged in correctly and try booting into DFU mode again</ins>.
_________________________________________________________
Exit katapult:
```
cd
```
And enter Kiauh again:
```
./kiauh/kiauh.sh
```
Go to the advanced menu.

Then, "Find MCU ID" and choose "USB (DFU)".

The ID of the board will be displayed (e.g. 0483:df11), copy it to a notepad. You can exit Kiauh.

**This part is important, pay attention**

*For STM32 boards:*
```
sudo dfu-util -R -a 0 -s 0x08000000:leave -D ~/katapult/out/katapult.bin -d 0483:df11
```
Change the 0483:df11 to your ID.

*For RP2040 boards:*
```
cd ~/katapult
make flash FLASH_DEVICE=0483:df11
```
Change the 0483:df11 to your ID.

Now press RESET on the board.
_________________________________________________________
To check if the flashing was successful, run:
```
ls /dev/serial/by-id/
```
It should show a usb-katapult... device.
Copy that whole id, from the usb to if00, to a notepad.

PUT A PIC HERE
_________________________________________________________
## Klipper
### Compile Klipper
Kiauh provides us an easy way to manage everything Klipper! :D

Enter Kiauh (do you remember the command?)

Go to advanced and "Build only"

*Some of the most common board compiles are in the Klipper folder in Board-Builds*

When you are done, exit out (Q) and wait for it to finish.

Now you can exit Kiauh.
_________________________________________________________
### Flash Klipper
First, run this to stop klipper from interfering with our flash:
```
sudo service klipper stop
```
<ins>**Important:**</ins> Double press the RESET button to make the board go into katapult boot mode.

Then, do the actual flash:
```
python3 ~/katapult/scripts/flashtool.py -f ~/klipper/out/klipper.bin -d /dev/serial/by-id/meow
```
But paste the ID you got from previous steps instead the "meow" :3

*Ignore the errors that will show up, if at the end it shows "Complete", everything is fine.*

Press RESET and run "ls /dev/serial/by-id/" to see if everything truly worked. You should now see a usb-Klipper... device.
_________________________________________________________
*Congrats, you just completed the hardest part of your software journey* :D

*You can sit back and relax as we glide through the rest of the guide.*

<sub>If any problems arise, don't hesitate to reach out to the guys (and gals) at the Voron Discord, Reddit, and other platforms. Or DM me directly through Discord: @Suzu_0071</sub>
