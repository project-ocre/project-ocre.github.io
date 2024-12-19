---
title: "Adding Board Support"
layout: default
parent: Board Support 
nav_order: 0
has_toc: false
---

# Adding Board Support

While we strive to support as many boards as possible in the Ocre project, the rapid pace of hardware innovation makes it impossible to maintain support for every device. This is where our community plays a crucial role in expanding the Ocre ecosystem by contributing board support for their specific hardware. 

This guide will walk you through the process of creating the necessary configurations that allow Ocre to understand and interact with your board's devices, peripherals, and unique capabilities.

---

**Before proceeding with adding board support, ensure:**
* Your board
  * Is [supported](https://docs.zephyrproject.org/3.7.0/boards/index.html) by Zephyr (v3.7.0)
  * Has a *minimum* of 550kb device memory
  * Has networking capabilities (either onboard or via external module)
* You can build the Ocre Runtime [Using a Simulated Device](../../quickstart/firmware/simulated). We will use this as our development environment and it will help us validate that your project is properly set up before adding support for physical hardware.
* You have a basic understanding of your board's hardware specifications and have access to your board's technical documentation

---

## Next Steps

Now that you have confirmed your development environment is working correctly and the Ocre Runtime can be ran with the native simulator, you're ready to begin adding support for your board. Continue to the [Create a Device Overlay](overlay) page to learn how to define your board's hardware configuration for the Ocre runtime.