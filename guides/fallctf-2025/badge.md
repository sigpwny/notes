---
title: Badge
---

# Fall CTF 2025 Badge

The Fall CTF 2025 badge is your all-in-one connectivity tool and gaming device for Fall CTF 2025. It contains an ESP32-S2 module with Wi-Fi, allowing for seamless communication between multiple badges. Each badge also has an accelerometer and joystick for interactive control. To power the device, each badge features a 400mAh 3.7V Li-po battery. The badge runs Micropython for ease-of-use and hacking.

## Connect your badge to your computer

### Physical setup

There is a USB-C port on the side of the badge, which is intended to connect to your computer. You may use any USB-C cable with data.

### Software setup

If you are on Linux, you may need to add your user to the `dialout` group to allow it to access the serial ports. Run the following command, then log out and log back in.
```sh
sudo usermod -a -G dialout $USER
```

If you are getting an error with `mpremote` command not found, try replace it with `python3 -m mpremote` (in all the commands below).

Install the `mpremote` Python package to connect to the badge:

```sh
pip install --user mpremote
```

From there, type `mpremote` to open a REPL on the badge. Press Ctrl-C to interrupt the current running program and Ctrl-D to reboot the badge. Press Ctrl-X to exit the prompt.

### Boot flag

To get the boot flag, connect the badge to the computer and run `mpremote`. Press Ctrl-C to interrupt the running program and Ctrl-D to reboot to get the boot flag.

## Badge hacking

Check out the src.zip for the source code. Run `mpremote ls` to list files and `mpremote cp :src.zip .` to copy files from the device (in this example, `src.zip`).

After modifying a `.py` file, Run `mpremote cp file.py :` to upload the updated code to the badge.

To reboot the badge after uploading a file, either

- Press the small button on the back left of the badge (next to the battery and the switch).
- Connect with `mpremote`, then press `Ctrl`+`D`. (If nothing happens, try pressing `Ctrl`+`C` first.)

If you are modifying a large file, such as `asteroids_game.py`, the device may complain about being out of memory. To resolve this, compile the file to bytecode using mpy-cross. Run `pip install mpy-cross` to install mpy-cross. Then, compile the file with `mpy-cross asteroids_game.py`. Finally, upload the file to the device with `mpremote cp asteroids_game.mpy :`.

Note that micropython python will load files in the following order: `.py` -> `.mpy` -> frozen files. By default, all our code is frozen. To override any module, simply include the .py or .mpy file in the top level directory. If there are both .py and .mpy files, the .py file will be used. The .mpy file uses significantly less memory, as micropython does not need to compile it.
