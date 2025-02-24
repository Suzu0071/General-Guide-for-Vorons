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
Enter it now, this is used to acces the Kiauh menu.
You can decline all of the update requests.

Now that Kiauh is installed, we can start downloading Klipper itself.

The order of downloading is Klipper, Moonraker, Mainsail/Fluidd. This is important since it can sometimes show errors (atleast in my experience).
_________________________________________________________
### Download/install Klipper
Navigating through Kiauh is done with number (you probably already figured that out). To install, choose "Install" on the main menu.

From the install menu, download Klipper.

*During the download, it will ask how many instances you want to set up. 1 should be the default.*

Installing Klipper may take a while depending on internet speeds and other factors. After it's installed, you should be directed back to the install menu, if you didn't get any errors, proceed to downloading Moonraker.
_________________________________________________________
### Download/install Moonraker
From the install menu, download Moonraker.

*During the download, it will ask for different parameters, unless you know what you're doing, it's recommended to keep them at default.*

After the installation, you'll be redirected to the install menu, if no errors showed up, proceed to the next step. 
_________________________________________________________
### Download/install 
**If you made more then 1 instance of klipper, you'll have to download Moonraker for all of the instances**
