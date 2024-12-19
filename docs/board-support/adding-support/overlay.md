---
title: "Create a Device Overlay File"
layout: default
parent: Adding Board Support 
nav_order: 0 
---

# Create a Device Overlay File

The Ocre runtime, built on the Zephyr RTOS, requires proper configuration of your board's components and memory layout. This guide will walk you through creating a device overlay file. 

---

## What is a Device Overlay File?

In Zephyr, each board has a default [Device Tree Source](https://docs.zephyrproject.org/latest/build/dts/intro-scope-purpose.html) (`.dts`) file that defines its hardware configuration, including memory mappings, peripherals, and bus configurations. An *overlay* file (`.overlay`)allows you to extend or modify this default configuration without changing the original source files.

For Ocre, proper overlay configuration is crucial for:
- Defining flash memory partitions for the boot-loader and containers
- Configuring board-specific hardware features
- Ensuring correct memory allocation for the runtime

---

## Creating Your Overlay File
Creating a device overlay file involves configuring your board's flash memory partitions, hardware peripherals, and possibly the filesystem for the Ocre Runtime to function properly. You'll need to understand your board's memory layout and hardware capabilities, as well as any additional modules you might need for networking support. Below you'll find requirements, examples, and step-by-step instructions to create your overlay.

### Prerequisites
Before creating your overlay file:

- Check out Zephyrs [Introduction to Devicetree](https://docs.zephyrproject.org/latest/build/dts/intro.html) guide.
- Locate your boards (Zephyr) **name**, as found in the Zephyr [Supported Boards](https://docs.zephyrproject.org/3.7.0/boards/index.html) guide. (e.g. `nucleo_h563zi`)
- Review your boards `.dts` file as found in the Zephyr source (`zephyr/boards/manufacturer/board_name/board_name.dts`).
- Review your board's technical documentation

{: .highlight}
The `board_name.dts` file provides a foundational reference for your board's hardware configuration but may not describe all available peripherals, hardware features, or supported options. Zephyr's Device Tree support is an evolving process and may not fully align with your board's latest specifications or capabilities. Use the `.dts` file as a baseline, but cross-reference with your board's datasheet and hardware documentation to ensure accuracy and completeness in your overlay file.


### Partition Layout Requirements
Ocre uses MCUBoot as a boot-loader, therefore it requires setting up the flash partitions accordingly. (See the [MCUboot docs](https://docs.mcuboot.com/readme-zephyr.html) for full details.)

| Partition | Size (min) | Description |
|----------|----------|----------|
| `boot_partition` | 32k | MCUboot boot-loader |
| `slot0_partition` | 256k | Primary slot for image 0 |
| `slot1_partition` | 256k | Secondary slot for image 0 |
| `storage_partition` | 8k | Storage for custom Zephyr functions |
| `user_data` | Remaining memory | Storage for Ocre container definitions and images |

{: .note}
Not all boards will require you to define *all* the above partitions in the `.overlay` file, as their `.dts` file may already define them. Please refer to your boards `.dts` file beforehand and only define (or modify) partitions if they're needed.

### Additional Device Tree Configurations

Beyond flash partitions, your board may require additional device tree configurations depending on its hardware and requirements.

Common configurations include:

- **File System Support**: Configure file system tables via `fstab` nodes for container storage
- **Chosen Nodes**: Specify system-wide defaults like console, shell, and flash devices
- **Hardware Peripherals**: Enable and configure communication interfaces like:
  - Ethernet modules (like the *enc28j60* example below)
  - WiFi modules (such as esp32 or other wireless adapters)
  - Serial interfaces and other board-specific peripherals

{: .note}
These configurations vary by board. If your board lacks built-in networking capabilities, you'll need to add support for an external ethernet or WiFi module as Ocre requires internet connectivity for container management.

### Example Overlay File 
Here's a sample overlay for STMicroelectronics `B-U585I-IOT02A` board:

```
&spi1 {
	enc28j60: ethernet@0 {
		status = "okay";
		reg = <0>;
		compatible = "microchip,enc28j60";
		int-gpios = <&gpioc 1 GPIO_ACTIVE_LOW>;
		spi-max-frequency = <10000000>;
		local-mac-address = [ 00 80 00 01 02 03 ];
		full-duplex;
	};
};

&flash0 {
	partitions {
		user_data_partition: partition@100000 {
			label = "user_data";
			reg = <0x00100000 DT_SIZE_K(256)>;
		};
	};
};
```

As mentioned in the previous section, many boards already have some partitions defined in their `.dts` file. This board only needed an additional `user_data_partition` definition in its overlay as the other required partitions were defined in its base [DTS file](https://github.com/zephyrproject-rtos/zephyr/blob/main/boards/st/b_u585i_iot02a/b_u585i_iot02a.dts).

You also may have noticed that this board also has an added configuration for the *enc28j60* ethernet controller as that board does not have an onboard ethernet or Wifi device. 

### Steps

Creating an overlay file will most likely take a bit of iteration as each board and requirements are unique. Also, depending on which options, devices, and peripheral you place in the overlay you may require additional configuration options to get your build to work so please see [**Creating a Device Config**](../config) to do that.

1. Create a blank file named `board_name.overlay` (replacing `board_name` with your boards name) in the *top-level* `boards` directory (`application/boards`).
2. Add the required partition layout to your overlay file, while referencing the [**Partition Layout Requirements**](#partition-layout-requirements) section and [**Example Overlay File**](#example-overlay-file) example above.
3. Save your overlay file.
4. Attempt to build the Ocre runtime following the [**Building and Flashing Ocre to Your Board**](../ocre-runtime) guide.
5. Most likely you will encounter errors, address them in the overlay (or in the [**config**](../config)) and try to build again.

---

## Next Steps
If your board builds successfully, congratulations! Continue to the [**Contributing Board Support**](../contributing) page to figure out how to get your board added to the official Ocre repos.

If you weren't successful by simply creating an overlay, then see the [**Create a Device Config**](../config) to add necessary configuration options for your board.