---
title: "B-U585I-IOT02A"
layout: default
parent: STMicroelectronics 
grandparent: Board Support
---

# B-U585I-IOT02A Discovery Board Overview

---

## Overview

The **STM32 B-U585I-IOT02A Discovery Board** is a versatile IoT development platform built around ST Micro's STM32U585AI microcontroller. This board combines ultra-low-power operation with advanced security features, making it ideal for developing IoT, wearable, and sensor applications.

Key features include:
* Arm® Cortex®-M33 processor with TrustZone® security
* 2 MB Flash memory and 786 KB SRAM
* Wi-Fi® (802.11 b/g/n) and Bluetooth® Low Energy connectivity
* Rich sensor array:
 * 3D accelerometer and gyroscope
 * Temperature and humidity sensor
 * Digital barometer
 * Time-of-flight and gesture detection
* Multiple expansion options via ARDUINO® Uno V3, STMod+, and Pmod™ connectors
* Integrated STLINK-V3E debugger with USB connectivity

In the diagram below, you can see many of the board's physical components:

![Board Layout](B-U585I-IOT02A.png)

---

## Powering the Device

There are two micro-USB ports on the board. The *top right* usb port labeled **USB STLINK** is the usb port we'll be using in this guide. Plug in the device and you should see some LEDs light up.

---

## Connecting to the Device

---

## Rebooting the Device

You may reboot the device by pressing the black button once.

---

## Resetting the Device to Factory Defaults

Some information may be stored to flash memory and persist across reboots, such as any WiFi credentials you may have entered or applications that have been installed. You can erase the flash memory to reset the board to a "Factory Default" state by pressing and holding the blue button for five seconds.

---

## Important Links

For additional details about the board and development tools, please refer to the following resources:

* **Product Page**: [https://www.st.com/en/evaluation-tools/b-u585i-iot02a.html](https://www.st.com/en/evaluation-tools/b-u585i-iot02a.html)
* **User Manual**: [https://www.st.com/resource/en/user_manual/um2839-discovery-kit-for-iot-node-with-stm32u5-series-stmicroelectronics.pdf](https://www.st.com/resource/en/user_manual/um2839-discovery-kit-for-iot-node-with-stm32u5-series-stmicroelectronics.pdf)