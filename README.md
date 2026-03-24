# M2rooter
A guide to root the Sunmi M2

Check the "Issues" tab for issue guidelines.

## Requirements
- A Linux computer with Python, Git, ADB, wget, and usbutils installed. (nothing for windows yet, don't use in a live USB!)
- A Sunmi M2 (obviously) and a working Internet connection.

## Guide
1. Run these commands to clone a cleaned-up version of B. Kerler's EDL utility (we will place the only needed loader later, don't worry):
```shell
git clone https://github.com/bkerler/edl.git
rm -rf edl/Loaders
mkdir -p edl/Loaders
```

2. Run this command to get the EDL loader needed:
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

5. Run this command EXACTLY to get a backup of your boot image (DO NOT INTERRUPT THIS PROCESS):
```shell
python3 edl.py --memory eMMC --loader Loaders/edl.bin r boot boot.img
```

6. Exit EDL mode by holding the Power button for 15-20 seconds.

7. Switch your Sunmi M2 to USB file transfer mode in Android, then copy boot.img to the root of the M2's storage.

8. Download and install the Magisk app from here: https://github.com/topjohnwu/magisk/releases/latest
  
9. Open Magisk, click "Install" in the "Magisk" tab (NOT "App").

10. Select "Select and Patch a File", and select "M2" from the file selection menu. (If it doesn't appear, click the hamburger menu on the top left. If it still doesn't appear, close the Magisk app from the App Switcher and reopen it, if it still doesn't appear then open an issue.)

11. Click "boot.img", then "LET'S GO" (or similar)

12. Wait for the Magisk app to finish, it will now say "Wrote output file to /storage/emulated/0/magisk_patched-30700_GA6gi.img" or similar.

13. Run this command on your computer:
```shell
adb pull /sdcard/magisk_patched*.img ./
```

13. Run EXACTLY these commands to reboot back to EDL and flash the boot image:
```shell
adb reboot edl
python3 edl.py --memory eMMC --loader Loaders/edl.bin w boot magisk_patched*.img
[[ $? -eq 0 ]] && echo "Successful! Hold the power button for 15-20 seconds to exit EDL." || echo "Failed."
```

14. Exit EDL mode by holding the Power button for 15-20 seconds.

15. Done! You should now be rooted. 
