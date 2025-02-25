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
*Configure everything through the Katapult folder in Board Builds*

Press q to exit and choose to save. 