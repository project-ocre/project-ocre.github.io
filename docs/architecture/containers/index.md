---
layout: default
title: Ocre Containers 
has_children: true
parent: Architecture
nav_order: 1
has_toc: false
---

# Ocre Containers 

Ocre containers are lightweight, portable environments that allow software to run consistently across different platforms. In the embedded world, Ocre containers bring the benefits of containerization to resource-constrained devices, revolutionizing embedded development. Ocre containers enable developers to write applications in any language that can compile to WebAssembly, including C, C++, Rust, and more. This flexibility allows organizations to easily repackage existing C code into containerized apps, significantly simplifying management compared to monolithic firmware.

![](ocre_containers.webp)

The containerized approach facilitates easier collaboration in embedded projects, allowing vendors to contribute modules without deep integration challenges. Moreover, containers provide enhanced security through virtualization, which is particularly crucial for tiny devices lacking hardware memory management units. Importantly, Ocre containers offer these benefits while maintaining near-native performance, making them suitable for a wide range of embedded applications.

---

## Learn More

Continue to the [Image Format](../containers/image_format) page to understand how Ocre implements the OCI container specification for embedded devices, then explore the individual container components in detail.