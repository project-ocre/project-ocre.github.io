---
title: Using a Simulated Device 
layout: default
parent: Building and Flashing the Ocre Runtime 
nav_order: 0 
---

# Using a Simulated Device

This guide covers building and running the Ocre runtime using Zephyr's `native_sim` target - a simulated environment that allows you to test Ocre applications without physical hardware. This approach is particularly useful for initial development and testing.


{: .important}
> There are *two* key limitations when building the native simulator:
> 1. Building on ARM architectures is not currently supported due to a lack of 32-bit support.
> 2. The POSIX architecture does not work on macOS due to fundamental differences between macOS and other Unix-like operating systems.
> 
> For users affected by these limitations, we recommend using one of the following alternatives:
> - A Linux-based x86_64 development container. (Feel free to use the [Ocre Dev Container](../../prerequisites/dev-environment/index) you obtained when setting up your dev environment.)
> - A computer with a x86_64 architecture 
> - Emulating a Linux-based VM with x86_64 architecture

---

## Steps

### **1. Install Dependencies and Zephyr SDK**

Complete the [Install dependencies](https://docs.zephyrproject.org/3.7.0/develop/getting_started/index.html#install-dependencies) and the [Install the Zephyr SDK](https://docs.zephyrproject.org/3.7.0/develop/getting_started/index.html#install-the-zephyr-sdk) sections for your host operating system from the Zephyr (v3.7.0) [Getting Started Guide](https://docs.zephyrproject.org/3.7.0/develop/getting_started/index.html#getting-started-guide). 

{: .note}
Currently the Ocre runtime is built on Zephyr `3.7.0`. As a result, the links in the above section reference `v3.7.0` documentation and not the *latest* documentation. It is advised to follow the links in this guide to ensure compability with your device.

### **2. Install WEST**

Install the [west](https://docs.zephyrproject.org/latest/develop/west/index.html) CLI tool, which is needed to build, run and manage Zephyr applications.

```
pip install west
```

{: .note}
We recommend using a Python virtual environment like [venv](https://docs.python.org/3/library/venv.html) to help manage pip dependencies for this project.

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

### **5. Build the application**

The following will build the firmware for the *virtual*, `native_sim` target which will allow you to run the Ocre runtime on a simulated device, rather than a physical board.
```
west build -b native_sim ./application -d build -- -DMODULE_EXT_ROOT=`pwd`/application
```

### **6. Run the application**

To run the application, simply run the following command:
```
./build/zephyr/zephyr.exe
```