# Preface

SIGPwny's [[badge|Fall CTF 2025 badge]] for the first time featured a fully custom board and comes with an [ESP32-S2-SOLO-2](https://www.espressif.com/sites/default/files/documentation/esp32-s2-solo-2_esp32-s2-solo-2u_datasheet_en.pdf) microcontroller. (Previous badges simply used a [Raspberry Pi Pico](https://www.raspberrypi.com/products/raspberry-pi-pico/), which featured the [RP2040](https://www.raspberrypi.com/products/rp2040/) microcontroller.)

During Fall CTF 2025, badges were distributed with FreeRTOS, running [MicroPython](https://micropython.org/) as a task to execute the programs built for Fall CTF. This was done to make the badge easily hackable, as editing Python code is simple and does not require a build process since MicroPython is a runtime and interprets Python code on the fly.

However, all these layers of abstraction means that the badge is not very efficient. The overhead of MicroPython and FreeRTOS doesn't leave the programmer with a lot of memory to use.

As part of Embedded Team, we want to develop closer to the bare metal to get a better understanding of embedded systems development and the nuances of hardware attacks. Having access to a microcontroller to do bare-metal development is essential - and since we've given out a bunch of these badges for free, we have created this guide to get you started with programming the ESP32 MCU, along with the rest of the peripherals on your badge!

# Coming soon!