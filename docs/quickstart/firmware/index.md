---
title: Building and Flashing the Ocre Runtime
layout: default
parent: Quickstart
nav_order: 1 
has_children: true 
---

# Building and Flashing the Ocre Runtime

This section will guide you through the process of building the Ocre runtime from source. Whether you're using a simulated environment for testing or deploying to physical hardware, the initial setup and build steps are identical. The paths diverge only at the final stage: simulated devices simply run the runtime directly, while physical devices require an additional flashing step to load the runtime onto the board. 

{: .note}
To streamline your development workflow, check out our section on [Setting Up Your Development Environment](dev-environment) just before continuing. There, we walk you through using the "Ocre Workspace" file provided in our repository. This VS Code workspace file includes preconfigured `west` build commands for all supported boards, along with debugging utilities and other helpful settings to make building and flashing the Ocre runtime easier.

---

## Sample App

To help you get started, we've included a sample application (`./src/main.c`) that demonstrates basic Ocre runtime usage by writing a simple hello world application to flash and directing the runtime to load and execute it.