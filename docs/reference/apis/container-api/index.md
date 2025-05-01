---
title: Container
layout: default
parent: APIs 
has_toc: false 
---
# Container APIs

Container APIs provide direct interfaces for containers to efficiently interact with underlying system resources and capabilities. Designed with simplicity, security, and consistency in mind, these APIs follow well-defined patterns to facilitate ease of use and seamless integration.

---

## Available APIs

Below are the core APIs available for Ocre containers. Each API includes detailed documentation with usage examples, error handling patterns, and best practices.

### Communication

Communication APIs enable containers to interact with each other and with external systems.

| API | Description|
|-----|-------------|
| **[Inter-Container Messaging](inter-container-messaging)** | Enable secure communication between containers |

## Hardware

Hardware APIs provide direct access to physical devices and sensors attached to the system.

| API | Description |
|-----|-------------|
| **[GPIO](gpio)** | Control digital pins for input/output operations with hardware peripherals and external components |
| **[Sensors](sensors)** | Interface with hardware sensors for environmental and motion data |

### Time Management

Time management APIs enable precise timing operations for container applications.

| API | Description |
|-----|-------------|
| **[Timers](timers)** | Create and manage timed events with millisecond precision |