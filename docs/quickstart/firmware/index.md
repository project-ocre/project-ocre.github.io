---
title: Building and Flashing the Ocre Runtime
layout: default
parent: Quickstart
nav_order: 1 
has_children: true 
---

# Building and Flashing the Ocre Runtime

This section will guide you through the process of building the Ocre runtime from source. Whether you're using a simulated environment for testing or deploying to physical hardware, the initial setup and build steps are identical. The paths diverge only at the final stage: simulated devices simply run the runtime directly, while physical devices require an additional flashing step to load the runtime onto the board. 

To help you get started, we've included a sample application (`./src/main.c`) that demonstrates basic Ocre runtime usage by writing a simple hello world application to flash and directing the runtime to load and execute it. Choose your preferred development path below to begin.