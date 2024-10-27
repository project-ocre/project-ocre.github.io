---
title: Using a Physical Device 
layout: default
parent: Building and Flashing the Ocre Runtime 
nav_order: 1 
---

# Using a Physical Device

This guide covers building and flashing the Ocre runtime onto actual development boards. While simulated environments are great for initial testing, deploying to real hardware allows you to test your applications under authentic conditions and take advantage of board-specific features like sensors and networking capabilities.

Please refer to the board-specific documentation in our [Board Support](../../../board-support) section for detailed setup instructions and requirements for your board before attempting to flash Ocre.

---

## Steps

### **1. Install Dependencies and the Zephyr SDK**

Complete the [Install dependencies](https://docs.zephyrproject.org/latest/develop/getting_started/index.html#install-dependencies) section from the Zephyr [Getting Started Guide](https://docs.zephyrproject.org/latest/develop/getting_started/index.html#getting-started-guide). 

{: .note}
For the following steps we recommend using a Python virtual environment like [venv](https://docs.python.org/3/library/venv.html).

### **2. Install WEST**

Install the [west](https://docs.zephyrproject.org/latest/develop/west/index.html) CLI tool, which is needed to build, run and manage Zephyr applications.

```
pip install west
```

### **3. Initialize the workspace**

Next, we will prepare the Zephyr workspace and checkout the project code.

First, create the `runtime` directory in the location of your chosing.
```
mkdir runtime
```
Next, cd to the `runtime` directory.

```
cd runtime
```

Now, initialize the `ocre-runtime` repo.
```
west init -m git@github.com:project-ocre/ocre-runtime.git
```

Lastly, update the repo with the `west` utility.

```
west update
```

### **4. Install Additional Zephyr (pip) requirements**

In order to build the Ocre runtime properly, you'll need to install a few remaining requirements for Zephyr.

```
pip install -r zephyr/scripts/requirements.txt
```

### **5. Install the Zephyr SDK**
For the last Zephyr requirement, we must install the SDK. 

```
cd zephyr/
west sdk install && cd ..
```

### **5. Build the Ocre Runtime**

To build and flash for a physical device, follow these steps:

1. Connect your board to your computer.

2. Build the application for your specific board. Replace `BOARD_NAME` with your board's name:
   ```
   cd ..
   west build -b BOARD_NAME ./application -d build -- -DMODULE_EXT_ROOT=`pwd`/application
   ```

{: .note}
See the list of [supported boards](https://docs.zephyrproject.org/latest/boards/index.html) from Zephyr to gather your board name. Or, simply run `west boards` from the terminal.


### **6. Flash the Ocre Runtime to Your Device**
1. Flash the application to your device:
   ```
   west flash
   ```

2. After flashing, restart/reset your board to run the application.

## Troubleshooting
This section covers common issues you might encounter when building and flashing the Ocre runtime to physical hardware, along with their solutions.

- **Unsupported Board**: If you're unable to flash to your board, and it's not listed in our [supported boards](../../../board-support), check out our [Adding Board Support](../../../board-support/adding-support) guide under the Board Support section. This guide will walk you through the process of adding support for your specific hardware. 