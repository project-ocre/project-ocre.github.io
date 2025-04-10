---
title: Hello World 
layout: default
nav_order: 0 
parent: Sample Applications
has_children: true 
---

# Hello World Sample

Learn how to *build* and *deploy* your first Ocre container using our Hello World sample. This guide demonstrates the fundamental workflow of containerized application development with Ocre.

---

## Building the Hello World Sample

The following steps will guide you to build the Hello World sample.

**1.** Move into the `hello-world` directory.
```
cd samples/hello-world
```

**2.** Next, build the hello world sample using the following commands:
```
mkdir build && cd build
cmake ..
make
```

This step results in the creation of a [WASM](https://webassembly.org/) file named `hello-world.wasm`, which will be used by the Ocre CLI to create an Ocre container in the next step.

**3.** Last, lets create an Ocre container using the Ocre CLI.

{: .important}
Steps to be updated soon

___

## Deploying the Hello World Sample to Your Device

Now that you've built your container, let's get it running on your device.

{: .important}
Steps to be updated soon

---