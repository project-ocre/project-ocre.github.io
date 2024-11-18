---
title: Building And Deploying Your First Container 
layout: default
parent: Your First Container
nav_order: 1

nav_exclude: false 
search_exclude: true
---

# Building and Deploying Your First App

Now that you have set up your development environment, you're ready to build and deploy your first container-based application with Ocre. This guide will walk you through the process of building a sample application from our getting-started repository.

---

## Prerequisites

Before proceeding, ensure you have:
- Completed the [Setting Up Your Development Environment](../dev-environment) guide
- Cloned the [`getting-started`](https://github.com/project-ocre/getting-started) repository

---

## Building Your First Container

Let's build your first Ocre container using one of our sample applications.

1. **Navigate to the Samples Directory**
  ```
  cd getting-started/samples
  ```
2. **Choose a Sample Application:** Browse through the available samples and choose one that interests you. Each sample demonstrates different capabilities of the Ocre platform.

3. **Create and Enter Build Directory:** Once you have jumped into the sample of your choosing, lets create the build directory.
```
mkdir build && cd build
```

4. **Configure the Build**
```
cmake -DWASI_SDK_DIR=/opt/wasi-sdk \
      -DCMAKE_TOOLCHAIN_FILE=/opt/wamr-sdk/app-sdk/wamr_toolchain.cmake ..
```

5. **Build the Container**
```
make
```

After successful compilation, you'll find a `sample.nbx` file in your build directory (where "sample" matches your chosen sample name). This is your containerized application, ready for deployment.

{: .important}
If the build fails, ensure you're running these commands within the dev container or interactive Docker environment, not on your host system.


## Deploying Your First Container

Now that you've built your container, let's get it running on your device.

{: .important}
Steps to be updated soon

---

## What's Next?
Now that you've successfully built your first container, you can:

- Explore other [samples](../../../samples) to learn more about Ocre's capabilities
- Learn how to create your own container from scratch using the [Ocre Container APIs](../../../reference/container-api)