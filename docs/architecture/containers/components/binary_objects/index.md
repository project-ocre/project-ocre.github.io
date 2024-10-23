---
layout: default
title: Binary Objects (Optional)
parent: Components
nav_order: 2 
---

Ocre containers can optionally include binary objects as part of the container image. These are additional resources that can be referenced by the WebAssembly module during execution.

Common use cases for binary objects include:

* Storing images or sound files
* Incorporating pre-trained AI/ML models
* Including any other form of binary data required by the application

This feature allows developers to package necessary non-executable resources alongside their application code, enhancing the container's functionality and self-sufficiency while maintaining a clear separation between executable code and static data.