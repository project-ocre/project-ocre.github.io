---
title: "NUCLEO-H723ZG"
layout: default
parent: STMicroelectronics 
grandparent: Board Support
---

# NUCLEO-H723ZG Development Board

---

## Overview

The **STM32 NUCLEO-H723ZG Development Board** is a high-performance platform built around ST Micro's STM32H723 microcontroller. This board features the powerful Arm® Cortex®-M7 core making it ideal for computationally intensive applications requiring real-time processing.

Key features include:
* Arm® Cortex®-M7 processor running at up to 550 MHz
* 1 MB Flash memory and 564 KB SRAM
* ST-LINK/V3E embedded debug tool
* Ethernet connectivity support
* USB OTG ports
* Arduino Uno V3 connectivity
* ST morpho expansion pins
* Three ST Zio connectors
* Flexible power supply options:
 * ST-LINK USB VBUS
 * USB power supply
 * External sources

 In the diagram below, you can see many of the board’s physical components:

![Board Layout](NUCLEO-H723ZG.png)

---

## Connecting to the Device

### Physical Connection
The board has two micro-USB ports. Connect a micro-USB cable to the port labeled **ST-Link micro USB Connector (CN1)** in the top right corner of the board. Once connected, power and debug LEDs should illuminate, indicating the board is ready for use.

### Serial Connection
To monitor your device's output:
1. Open your preferred serial terminal application 
2. Configure the following settings:
  * **Port**: Select the port where your device is connected
  * **Baud Rate**: 115200
  * **Data Bits**: 8
  * **Parity**: None
  * **Flow Control**: None
  * **Line Endings**: None

{: .note}
The device will appear as a USB serial port when properly connected. On Linux/macOS it will typically be `/dev/ttyACM0` or similar, on Windows it will be a COM port.

---

## Flashing the Firmware

To flash the Ocre runtime, please follow the steps in the [Using a Physical Device](../../../quickstart/firmware/hardware) section found in the [Building and Flashing the Ocre Runtime](../../../quickstart/firmware) guide. 

{: .note}
Be sure to use `nucleo_h723zg` as your board name.

---

## Rebooting the Device

You may reboot the device by pressing the black button once.

---

## Resetting the Device to Factory Defaults
To erase all stored data and reset the board to its factory default state:

1. Open STM32CubeProgrammer
2. In the port dropdown menu, select "ST-LINK". Click the refresh icon next to "Serial Number", select your device, and click the green "Connect" button
3. Select "Full chip erase" from the left sidebar (eraser icon)
4. Click "OK" to confirm
5. A dialog box will confirm successful flash memory erasure
6. Click the green "Disconnect" button to free up the port for future flashing and debugging

**Note**: This will erase all stored data including WiFi credentials and installed applications.

---

## Important Links

For additional details about the board and development tools, please refer to the following resources:

* **Product Page**: [https://www.st.com/en/evaluation-tools/b-u585i-iot02a.html](https://www.st.com/en/evaluation-tools/b-u585i-iot02a.html)
* **User Manual**: [https://www.st.com/resource/en/user_manual/um2839-discovery-kit-for-iot-node-with-stm32u5-series-stmicroelectronics.pdf](https://www.st.com/resource/en/user_manual/um2839-discovery-kit-for-iot-node-with-stm32u5-series-stmicroelectronics.pdf)