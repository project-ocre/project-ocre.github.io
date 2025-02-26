---
title: Container APIs
layout: default
parent: Reference
has_toc: false 
---

# Container APIs

 Container APIs provide direct interfaces for containers to efficiently interact with underlying system resources and capabilities. Designed with simplicity, security, and consistency in mind, these APIs follow well-defined patterns to facilitate ease of use and seamless integration.

These APIs serve as the foundation for container interactions, enabling capabilities such as:

- **Hardware Resource Access:** Interface with sensors to collect environmental or motion data.
- **Timers:** Create and manage timed events with precise millisecond accuracy.
- **WebAssembly Integration:** Ensure smooth execution within a WebAssembly-based environment, abstracting the complexity from container applications.

---

## Available APIs

Below are the core APIs available for Ocre containers. They are categorized for your convenience, with detailed documentation for each, including usage examples, error handling patterns, and best practices.

### System Interaction
- [Sensors](sensors): Interface with hardware sensors for environmental and motion data
- [Timers](timers): Create and manage timed events with millisecond precision