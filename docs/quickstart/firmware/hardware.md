---
title: Using a Physical Device 
layout: default
parent: Building and Flashing the Ocre Runtime 
nav_order: 1 
---

# Building the Ocre Runtime

Building the Ocre runtime is the first step in creating a containerized app for your embedded device. This guide will walk you through the process of building the runtime from source and flashing it to your physical development board. 

Also, you will find a sample application (`./src/main.c`) that will demonstrate how to use the Ocre runtime, which writes a simple hello world application to flash and directs the Ocre runtime to load and execute it.

---

## Steps

### **1. Install Dependencies and the Zephyr SDK**

Complete the [Install dependencies](https://docs.zephyrproject.org/latest/develop/getting_started/index.html#install-dependencies) and the [Install the Zephyr SDK](https://docs.zephyrproject.org/latest/develop/getting_started/index.html#install-the-zephyr-sdk) sections from the Zephyr [Getting Started Guide](https://docs.zephyrproject.org/latest/develop/getting_started/index.html#getting-started-guide). There are several steps that must be performed if this is the first time you’ve developed for Zephyr on your machine.
In general, builds should be done on a Linux machine or on Windows using WSL2.

**Note:** For the following steps we recommend using a Python virtual environment like [venv](https://docs.python.org/3/library/venv.html).

### **2. Install WEST**

Install the [west](https://docs.zephyrproject.org/latest/develop/west/index.html) CLI tool, which is needed to build, run and manage Zephyr applications.

```
$ pip install west
```

### **3. Initialize the workspace**

This will checkout the project code and configure the Zephyr workspace.
```
$ cd

$ mkdir runtime

$ cd runtime

$ west init -m git@github.com:project-ocre/ocre-runtime.git

$ west update
```

### **4. Install Additional Zephyr (pip) requirements**

In order to build the Ocre runtime properly, you'll need to install a few remaining requirements for Zephyr.

```
pip install -r zephyr/scripts/requirements.txt
```

**Note:** This step is only possible after updating the repo with `west update`.

### **5. Build the Ocre Runtime**

To build and flash for a physical device, follow these steps:

1. Connect your board to your computer.

2. Build the application for your specific board. Replace `BOARD_NAME` with your board's name:
   ```
   $ west build -b BOARD_NAME ./application -d build -- -DMODULE_EXT_ROOT=`pwd`/application
   ```
**Note**: See the list of [supported boards](https://docs.zephyrproject.org/2.6.0/boards/index.html) from Zephyr to gather your board name. Or, simply run `west boards` from the terminal.


### **6. Flash the Ocre Runtime to Your Device**
1. Flash the application to your device:
   ```
   $ west flash
   ```

2. After flashing, restart/reset your board to run the application.