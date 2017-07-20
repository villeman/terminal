# Terminals

## Installation

1. Download [FullPageOS](http://docstech.net/FullPageOS/2017-06-24_2017-06-21-fullpageos-jessie-lite-0.7.0.zip) (direct download link).
2. Unzip the image and "burn" it to an SD card [like any other Raspberry Pi image].(https://www.raspberrypi.org/documentation/installation/installing-images/README.md)
3. Configure WiFi connection by editing `fullpageos-network.txt` on the root of the flashed card when using it like a flash drive.
4. Put SD card in Pi and connect Pi to network cable and power adapter.
5. Log into Pi via SSH: `ssh pi@fullpageos.local` (default password is "raspberry").

## Configuration

### raspi-config settings
1. Run `sudo raspi-config`.
2. Select option 1 and change password to something other than "raspberry" (you need to relaunch raspi-config after this step)
3. Select option 2 and set hostname of machine to `t1`. (For additional terminals set hostname to `t2`, `t3`, etc.)
4. Select option 4, then on the next screen option 1 and then locate `sv_SE.UTF-8 UTF-8` in the list, press <kbd>Space</kbd> to select it and then <kbd>Return</kbd> to continue. When prompted on the next screen, choose `en_GB.UTF-8` as the system default.

### Installing node.js and Node RED

1. Run the following command to automatically install both node.js and Node RED: `bash <(curl -sL https://raw.githubusercontent.com/node-red/raspbian-deb-package/master/resources/update-nodejs-and-nodered)` (this process will take 10-20 minutes)
2. Set Node RED to autostart: `sudo systemctl enable nodered.service`

### Set FullPageOS browser URL

Open `/boot/fullpageos.txt` with vi, vim or nano and replace URL in file with: `http://t1.local:1880/chat?id=1`. For any additional terminals, change `?id=1` to `?id=2`, `?id=3`, etc.

### Set keyboard layout to Swedish and disable modifier keys

Run `sudo nano /etc/default/keyboard` and set `XKBLAYOUT="se"` (<kbd>Ctrl</kbd>+<kbd>X</kbd> and then <kbd>Y</kbd> and <kbd>Return</kbd> to save).

Then also run `sudo nano ~/.profile` and add `setxkbmap se` to the bottom of the file and save.

To disable modifier keys and prevent user from using keyboard shortcuts like <kbd>Ctrl</kbd>+<kbd>W</kbd> to close the tab, run `sudo nano /usr/share/X11/xkb/symbols/pc` to open default keymap file. Then set the value inside brackets for the following lines to `NoSymbol`:

```
key <LCTL> {        [ Control_L             ]       };
key <LWIN> {        [ Super_L               ]       };

key <RTSH> {        [ Shift_R               ]       };
key <RCTL> {        [ Control_R             ]       };
key <RWIN> {        [ Super_R               ]       };
```
Like this:
```
key <LCTL> {        [ NoSymbol              ]       };
key <LWIN> {        [ NoSymbol              ]       };

key <RTSH> {        [ NoSymbol              ]       };
key <RCTL> {        [ NoSymbol              ]       };
key <RWIN> {        [ NoSymbol              ]       };
```

### Import chat terminal flow to Node RED and start chat server

1. Open a browser on a computer on the same network as the Pi and go to http://t1.local:1880.
2. Click the "hamburger" menu in top right, point to *Import* and then click *Clipboard*.
3. Copy the contents of t1.flow and paste it in the text area. Click *Import* and click anywhere in the flow to drop all drop all the imported nodes into the flow.
4. Click *Deploy* to deploy the code. Done!
5. SSH into the PI and run command `rm -rf ~/.config/chromium/Singleton*` to prevent "profile in use" message from Chromium. Then reboot the Pi with `sudo reboot` and the Pi should restart into the chat page, ready to go. That's all!
