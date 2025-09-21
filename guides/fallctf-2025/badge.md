---
title: Badge
---
# Fall CTF 2025 Badge

The Fall CTF 2025 badge is your all-in-one connectivity tool and gaming device for Fall CTF 2025. It contains an ESP32-S2 module with Wi-Fi, allowing for seamless communication between multiple badges. Each badge also has an accelerometer and joystick for interactive control. To power the device, each badge features a 400mAh 3.7V Li-po battery. The badge runs Micropython for ease-of-use and hacking.

## Connect your badge to your computer

### Physical setup

There is a USB-C port on the side of the badge, which is intended to connect to your computer. You may use any USB-C cable with data.

### Software setup

Install the `mpremote` Python package to connect to the badge:
```sh
pip install --user mpremote
```

From there, type `mpremote` to open a REPL on the badge. Press Ctrl-C to interrupt the current running program and Ctrl-D to reboot the badge. Press Ctrl-X to exit the prompt.

### Boot flag

To get the boot flag, connect the badge to the computer and run `mpremote`. Press Ctrl-C to interrupt the running program and Ctrl-D to reboot to get the boot flag.

## Badge hacking

Check out the src.zip for the source code. Run `mpremote ls` to list files and `mpremote cp :src.zip .` to copy files from the device (in this example, `src.zip`).
