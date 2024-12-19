---
title: "Building and Flashing Ocre to Your Board"
layout: default
parent: Adding Board Support 
nav_order: 2
---

# Building and Flashing Ocre to Your Board

Now that you've created your board's overlay and configuration files, you're ready to build and flash the Ocre Runtime. This page provides the commands and steps needed to get Ocre running on your device. You'll likely reference this page frequently while iterating through your overlay and config file development.

---


## Prerequisites
Before attempting to build and flash the Ocre Runtime to your device ensure the following conditions are met:
- Your *overlay* and *config* files have been created and placed in the top-level boards directory (`application/boards`). 
- Your board is connected via USB to your computer and connected via your console program.

---

## Building the Ocre Runtime

The build process will automatically incorporate your board's overlay and config files from the boards directory. If any configuration issues exist, the build will fail with helpful error messages indicating what needs to be fixed.

To **build** the Ocre Runtime:

1. Ensure you're in the projects root directory
2. Run the following command, specifying your `board_name` as the target:

```sh
west build -b board_name ./application -d build -- -DMODULE_EXT_ROOT=pwd/application
```

{: .note}
Find your exact board name in the Zephyr (v3.7.0) [Supported Boards Guide](https://docs.zephyrproject.org/3.7.0/boards/index.html).

---

## Flashing the Ocre Runtime
Once built successfully, you can flash the Ocre Runtime to your device. 

1. To **flash** the Ocre Runtime to your device, simply run:

```sh
west flash
```

If successful, you should see the following output in your console.

![](../success.png)

---

## Next Steps
Once you've successfully built and flashed the Ocre Runtime to your board, continue to the [**Contributing Board Support**](../contributing) page to learn how to contribute your board configuration to the Ocre Runtime repo. 