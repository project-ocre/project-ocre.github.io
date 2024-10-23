---
title: Sample Applications
layout: default
nav_order: 4 
has_children: true 
---

# Sample Applications

This page provides an overview of the sample applications available for Ocre-enabled devices. These samples demonstrate how to build and deploy applications using the Ocre platform, helping you get started with development quickly and efficiently.

---
## Sample Structure

Each sample project in this repository consists of the following components:

1. **Application source file** (`main.c`): Contains the main logic of the sample application in C.
2. **CMakeLists.txt** (`CMakeLists.txt`): Specifies how to build the project using CMake.
3. **Build configuration file** (`build.yaml`): Defines the container image configuration parameters. 
5. **Container application file** (`manifest.yaml`): Defines container application configuration parameters.

These files work together to create a complete Ocre container application.

---

## Available Samples
We offer *two* sample applications to help you understand and explore the capabilities of the Ocre platform:

1. **[Hello World](./hello-world/index.md):** A basic application that demonstrates the fundamental structure of an Ocre application and familiarizes you with the development environment.

2. **[Blinky](./blinky/index.md)**: An application that shows how to interact with device hardware by controlling an LED, introducing basic I/O operations on Ocre-enabled devices.

---

## Accessing the Samples

To access and work with the samples:

1. Open a terminal or command prompt.
2. Clone the [getting-started](https://github.com/project-ocre/getting-started) repo.
    ```bash
    git clone git@github.com:project-ocre/getting-started.git
    ```

3. Navigate to the samples directory:
    ```bash
    cd ./samples/
    ```
From here, you can explore each sample's directory to view the source code and follow the individual build and deployment instructions on the following pages.