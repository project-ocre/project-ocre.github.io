---
title: Building And Deploying Your First Container 
layout: default
parent: Your First Container
nav_order: 1
---

# Building and Deploying Your First App

Now that you have set up your development environment, you're ready to build and deploy your first container-based application with Ocre. 

---

## Prerequisites

Before proceeding, ensure you have:
- Completed the [Setting Up Your Development Environment](../dev-environment) guide

---

## Building Your First Container

Let's build your first Ocre container using the "*hello-world*" sample application.

1. **Navigate to *hello-world* sample in the *samples* directory**
  ```
  cd samples/hello-world
  ```

2. **Build the WASM Module**
  ```
  mkdir build && cd build
  cmake ..
  make
  ```

This step results in the creation of a file named `sample_name.wasm`, which will be used by the Ocre CLI to create an Ocre container in the next step.

---

## Deploying Your First Container

Now that you've built your container, let's get it running on your device.

{: .important}
Steps to be updated soon

---

## What's Next?
Now that you've successfully built your first container, you can:

- Explore other [samples](../../../samples) to learn more about Ocre's capabilities
- Learn how to create your own container from scratch using the [Ocre Container APIs](../../../reference/container-api)