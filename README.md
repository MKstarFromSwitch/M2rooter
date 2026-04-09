# M2rooter
A guide to root the Sunmi M2

## WARNING
This can **brick your device** if done incorrectly.

- You are doing this on your own risk.
- I am not responsible for any damage.

## Issue Guidelines
Go to the Issues tab to see a template to use when opening an issue.

## Requirements
- A Linux computer with Python, Git, ADB, wget, and usbutils installed. (nothing for windows yet, don't use in a live USB - driver installs and files won't persist!)
- A Sunmi M2 (obviously)
- A working Internet connection

## Guide
1. Run these commands to clone a cleaned-up version of B. Kerler's EDL utility (we will place the only needed loader later, don't worry):
```shell
git clone https://github.com/bkerler/edl.git
rm -rf edl/Loaders
mkdir -p edl/Loaders
```

2. Run this command to get the EDL loader needed (the source of the loader is from this loader repo: <a href=https://github.com/bkerler/Loaders>Loaders</a>):
```shell
wget -O edl/Loaders/edl.bin https://raw.githubusercontent.com/MKstarFromSwitch/M2rooter/HEAD/edl.bin
```

3. Run these commands to enter EDL:
```shell
cd edl
adb reboot edl
```

4. Run these commands to install EDL drivers (continue after your system reboots):
```shell
./install-linux-edl-drivers.sh
sudo update-initramfs -u -k all
sudo reboot
```

5. Run this command to get a backup of your boot image (DO NOT INTERRUPT THIS PROCESS):
```shell
python3 edl.py --memory eMMC --loader Loaders/edl.bin r boot boot.img
```

6. Exit EDL mode by holding the Power button for 15-20 seconds.

7. Switch your Sunmi M2 to USB file transfer mode in Android, then copy boot.img to the root of the M2's storage.

8. Download and install the Magisk app from here: <a href=https://github.com/topjohnwu/magisk/releases/latest>Magisk</a>
  
9. Open Magisk, click "Install" in the "Magisk" tab (NOT "App").

10. Select "Select and Patch a File", and select "M2" in the file selection menu (your device's internal storage). (If it doesn't appear, click the hamburger menu on the top left. If it still doesn't appear, close the Magisk app from the App Switcher and reopen it, if it still doesn't appear then open an issue.)

11. Click "boot.img", then "LET'S GO" (or similar)

12. Wait for the Magisk app to finish, it will now say "Wrote output file to /storage/emulated/0/magisk_patched-30700_GA6gi.img" or similar.

13. Run this command to get the patched image from the device:
```shell
adb pull /sdcard/magisk_patched*.img ./
```

14. Run these commands to reboot back to EDL and flash the boot image:
```shell
adb reboot edl
python3 edl.py --memory eMMC --loader Loaders/edl.bin w boot magisk_patched*.img
[[ $? -eq 0 ]] && echo "Successful! Hold the power button for 15-20 seconds to exit EDL." || echo "Failed."
```

15. Exit EDL mode by holding the Power button for 15-20 seconds.

16. Done! You should now be rooted. To verify root, run this command: ```adb shell su``` (If you see the message "/system/bin/sh: su: not found", then your system was not rooted correctly.)
