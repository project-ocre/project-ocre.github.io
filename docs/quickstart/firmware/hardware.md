---
title: Using a Physical Device 
layout: default
parent: Building and Flashing the Ocre Runtime 
nav_order: 2 
---

# Using a Physical Device

This guide covers building and flashing the Ocre runtime onto actual development boards. While simulated environments are great for initial testing, deploying to real hardware allows you to test your applications under authentic conditions and take advantage of board-specific features like sensors and networking capabilities.

{: .note}
Please refer to the "Building and Flashing the Ocre Runtime" section for your board in our in our [Board Support](../../../board-support) section for your boards name, as well as any additional setup instructions should you need them.

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
west init -m git@github.com:project-ocre/ocre-runtime.git
```

Lastly, update the repo with the `west` utility.

```
west update
```

{: .note}
`west update` may take 5-10 minutes as it pulls all Zephyr Project board sources.

### **5. Install Additional Zephyr (pip) requirements**

In order to build the Ocre runtime properly, you'll need to install a few remaining requirements for Zephyr.

```
pip install -r zephyr/scripts/requirements.txt
```

### **6. Build the Ocre Runtime**

Build the application for your specific board, replacing `BOARD_NAME` with your board's name:
```
west build -b BOARD_NAME ./application -d build -- -DMODULE_EXT_ROOT=`pwd`/application
```

{: .note}
You can also run the **West Build** task directly from the **Ocre Workspace** file as defined in the [Ocre Workspace File](../../../reference/workspacefile) reference. If you do, be sure to select your board as your build target from the dropdown list of available boards. 

### **7. Flash the Ocre Runtime to Your Device**

To flash the Ocre Runtime to your device follow these steps:

1. Connect your board to your computer.
2. Open your [serial console](../../quickstart/prerequisites/serial-console/index.md), and connect to your board.
1. Flash the application to your device:
   ```
   west flash
   ```

After flashing, restart/reset your board to run the application. If successful, you should see the following output on your console:

![](../success.png)

---

## Troubleshooting
This section covers common issues you might encounter when building and flashing the Ocre runtime to physical hardware, along with their solutions.

- **Unsupported Board**: If you're unable to flash to your board, and it's not listed in our [Board Support](../../../board-support), check out our [Adding Board Support](../../../board-support/adding-support) guide under the Board Support section. This guide will walk you through the process of adding support for your specific hardware. 