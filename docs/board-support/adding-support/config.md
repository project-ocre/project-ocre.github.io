---
title: "Create a Device Config File"
layout: default
parent: Adding Board Support 
nav_order: 1
---

# Create a Device Config File

The Ocre runtime requires specific configuration of your board's software features and hardware capabilities using Zephyr's Kconfig system. This guide will walk you through creating a device configuration file.

---

## What is a Device Config File?

In Zephyr, each board has default configurations defined in its defconfig file (`board_name_defconfig`). A device config file allows you to extend or override these default configurations without modifying the original files. While similar in purpose to Zephyr's standard Kconfig files, Ocre uses the `.conf` extension for board-specific configurations placed in the boards directory.

Config files are essential for:
- Enabling required hardware drivers and peripherals
- Setting memory and resource allocations
- Configuring networking and communication interfaces
- Enabling board-specific features needed by Ocre

---

## Creating Your Config File
Creating a device config file often goes hand-in-hand with developing your overlay file, as build errors will indicate missing configuration options needed for your board's hardware and features. The specific configurations needed will vary based on your board's capabilities and requirements.

### Prerequisites
Before creating your overlay file:
- Check out Zephyr's [Kconfig System](https://docs.zephyrproject.org/latest/develop/application/index.html#kconfig-configuration)
- Locate your boards (Zephyr) **name**, as found in the Zephyr [Supported Boards](https://docs.zephyrproject.org/3.7.0/boards/index.html) guide. (e.g. `nucleo_h563zi`)
- Review your board's default config in the Zephyr source (`zephyr/boards/manufacturer/board_name/board_name_defconfig`)
- Review board's technical documentation

{: .highlight}
While the `board_name_defconfig` file provides a useful reference for understanding your board's default configurations, it may not include all the board's features or capabilities. Zephyr board support is an ongoing effort and may lag behind the latest hardware revisions or available peripherals. Use the defconfig file as a starting point, but verify and supplement its configurations with your board's datasheet and hardware documentation as needed.


### Example Config
Here's a sample config for STMicroelectronics `B-U585I-IOT02A` board:

```
# ENC28J60 Ethernet driver
CONFIG_GPIO=y
CONFIG_SPI=y
CONFIG_ETH_ENC28J60=y
CONFIG_NET_L2_ETHERNET=y
```

This example shows configuration options needed for the *enc28j60* ethernet controller that we specified in the overlay for the `B-U585I-IOT02A` board in the previous page (See the [overlay example](../overlay#example-overlay-file)).

### Steps

1. Create a blank file named `board_name.conf` (replacing `board_name` with your boards name) in the *top-level* `boards` directory (`application/boards`) in the Ocre Runtime project. 
2. Add required configurations:
   - Enable necessary drivers and peripherals
   - Configure hardware features defined in your overlay
   - Set any board-specific options
3. Save your config file.
4. Attempt to build the Ocre Runtime following the [**Building and Flashing the Ocre Runtime to Your Board**](../ocre-runtime) guide.
5. As mentioned in the [**Create a Device Overlay File**](../overlay) page, most likely you will encounter errors, address them and try to build again.

{: .note}
Reference Zephyr's [Kconfig Search](https://docs.zephyrproject.org/latest/kconfig.html#kconfig-search) for available configuration options and their descriptions.

---

## Next Steps
If your board builds successfully, congratulations! You have successfully added board support for your device. Continue to the [**Contributing Board Support**](../contributing) page to learn how to get your board added to the official Ocre repos.