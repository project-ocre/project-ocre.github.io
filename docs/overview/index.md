---
layout: default
title: Overview 
has_children: false
nav_order: 1 
---
Atym is a SaaS-based infrastructure solution that extends containerization to billions of resource-constrained edge devices, breaking past the "Linux barrier" to enable containerized applications on devices with as little as 1MB of memory. It brings cloud-native principles and OCI-like containerization to embedded devices, allowing developers to build, deploy, and manage modular applications across a wide range of connected products.

![Atym High-level Architecture](atym_architecture.png)

Traditionally, updates on embedded devices have been risky and complex, requiring specialized tools like cross-compilers and unit test tools. Atym simplifies this process by leveraging [Zephyr RTOS](https://zephyrproject.org/), [WebAssembly](https://webassembly.org/), and its custom runtime. With Atym, developers can create containers using their preferred language, allowing for flexibility and accelerating the deployment of modular applications in secure, platform-independent containers. This approach reduces risk and enables safer, more frequent updates to edge devices.