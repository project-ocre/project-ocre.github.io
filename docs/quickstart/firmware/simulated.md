---
title: Using a Simulated Device 
layout: default
parent: Building and Flashing the Ocre Runtime 
nav_order: 0 
---

# Building the Ocre Runtime

Building the Ocre runtime is the first step in creating a containerized app for your embedded device. This guide will walk you through the process of building the runtime from source, including setting up the necessary development environment and tools, and running it on a simulated board to test functionality before deploying to physical hardware.

Also, there is a sample application (`./src/main.c`) that will demonstrate how to use the Ocre runtime, which writes a simple hello world application to flash and directs the Ocre runtime to load and execute it.

{: .note}
> There are *two* key limitations when building the native simulator:
> 1. Building on ARM architectures is not currently supported due to a lack of 32-bit support.
> 2. The POSIX architecture does not work on macOS due to fundamental differences between macOS and other Unix-like operating systems.
> 
> For users affected by these limitations, we recommend using one of the following alternatives:
> - A Linux-based x86_64 development container. (Feel free to re-use the [Ocre Dev Container](../prerequisites/dev-environment/index.md) you obtained when setting up your dev environment.)
> - A computer with a x86_64 architecture 
> - Emulating a Linux-based VM with x86_64 architecture

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

### **5. Build the application**

The following will build the firmware for the *virtual*, `native_sim` target which will allow you to run the Ocre runtime on a simulated device, rather than a physical board.
```
$ west build -b native_sim ./application -d build -- -DMODULE_EXT_ROOT=`pwd`/application
```

### **6. Run the application**

To run the application, simply run the following command:
```
$ ./build/zephyr/zephyr.exe
```