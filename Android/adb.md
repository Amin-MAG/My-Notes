# adb

```bash
# Install the adb tools
sudo apt install adb

# Connected devices
adb devices
```

## Push or Pull files

```bash
# To retrieve a file from the android device 
adb pull mnt/Downloads/example.apk

# To send a file to the android device 
adb push example.apk /mnt/Downloads/
```

## Shell

```bash
# To list all the installed packages and get the full paths
adb shell pm list packages -f

# Install an apk on host
adb shell install example.apk
# Or on the device
adb shell install /mnt/sdcard/Download/example.apk
```

# Resources
- [Android ADB Cheat Sheet](https://3os.org/android/adb-cheat-sheet/)