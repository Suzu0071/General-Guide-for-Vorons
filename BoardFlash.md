# Board Flashing and MCU IDs
_________________________________________________________
## Flash Katapult
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
