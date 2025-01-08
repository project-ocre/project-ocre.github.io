---
title: Using a Simulated Device 
layout: default
parent: Building and Flashing the Ocre Runtime 
nav_order: 1 
---

# Using a Simulated Device

This guide covers building and running the Ocre runtime using Zephyr's `native_sim` target - a simulated environment that allows you to test Ocre applications without physical hardware. This approach is particularly useful for initial development and testing.


{: .important}
> There are *two* key limitations when building the native simulator:
> 1. Building on ARM architectures is not currently supported due to a lack of 32-bit support.
> 2. The POSIX architecture does not work on macOS due to fundamental differences between macOS and other Unix-like operating systems.
> 
> For users affected by these limitations, we recommend using one of the following alternatives:
> - A Linux-based x86_64 development container. (Feel free to use the [Ocre Dev Container](../first-app/dev-environment) you obtained when setting up your dev environment.)
> - A computer with a x86_64 architecture 
> - Emulating a Linux-based VM with x86_64 architecture

---

## Steps

### **1. Install Dependencies and Zephyr SDK**

Complete the [Install dependencies](https://docs.zephyrproject.org/3.7.0/develop/getting_started/index.html#install-dependencies) and the [Install the Zephyr SDK](https://docs.zephyrproject.org/3.7.0/develop/getting_started/index.html#install-the-zephyr-sdk) sections for your host operating system from the Zephyr (v3.7.0) [Getting Started Guide](https://docs.zephyrproject.org/3.7.0/develop/getting_started/index.html#getting-started-guide). 

{: .note}
Currently the Ocre runtime is built on Zephyr `3.7.0`. As a result, the links in the above section reference `v3.7.0` documentation and not the *latest* documentation. It is advised to follow the links in this guide to ensure compatibility with your device.

### **2. Create a Virtual Python Environment (venv)**
Create a [Python virtual environment](https://packaging.python.org/en/latest/guides/installing-using-pip-and-virtual-environments/) (venv) to isolate your Python project dependencies.

```
mkdir runtime && cd runtime
python3 -m venv .venv
source .venv/bin/activate
```

{: .highlight}
You *may* need to install the `python3-venv` package (or equivalent) on your host system beforehand.

### **3. Install WEST**

Install the [west](https://docs.zephyrproject.org/latest/develop/west/index.html) CLI tool, which is needed to build, run and manage Zephyr applications.

```
pip install west
```

### **4. Initialize the workspace**

Now, initialize the `ocre-runtime` repo.
```
west init -m https://github.com/project-ocre/ocre-runtime.git 
```

Lastly, update the repo with the `west` utility.

```
west update
```

{: .note}
This step may take 5-10 minutes as it pulls all Zephyr Project board sources.

### **5. Install Additional Zephyr (pip) requirements**

In order to build the Ocre runtime properly, you'll need to install a few remaining requirements for Zephyr.

```
pip install -r zephyr/scripts/requirements.txt
```

### **6. Build the application**

The following will build the firmware for the *virtual*, `native_sim` target which will allow you to run the Ocre runtime on a simulated device, rather than a physical board.
```
west build -b native_sim ./application -d build -- -DMODULE_EXT_ROOT=`pwd`/application
```

{: .note}
You can also run the **West Build** task directly from the **Ocre Workspace** file as defined in the [Ocre Workspace File](../../../reference/workspacefile) reference. If you do, be sure to select `native_sim` as your build target from the dropdown list of available boards. 

### **7. Run the application**

To run the application, simply run the following command:
```
./build/zephyr/zephyr.exe
```

If successful, you should see the following output:

![](../success.png)

---

## Next Steps

Now that you have successfully built and flashed the Ocre runtime to your device, you're ready to create your first Ocre application. Head over to [Your First Container](../../first-app) to get started building containerized applications.