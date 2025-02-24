# Voron-Printer-Guide
Walk through guide relating most of the things Voron c:

_________________________________________________________
## Setting up Klipper, Moonraker, and Mainsail/Fluidd
Probably the easiest way to manage and download these is through [Kiauh](https://github.com/dw-0/kiauh). 
Here, we'll take a quick stroll through the installation and setup for the minimal required softwares to get your printer running.

### Download/install Kiauh
Get or update git:
```
sudo apt-get update && sudo apt-get install git -y
```
Then, download Kiauh itself:
```
cd ~ && git clone https://github.com/dw-0/kiauh.git
```
Now thats Kiauh is installed, you'll need to memorize one command:
```
./kiauh/kiauh.sh
```
