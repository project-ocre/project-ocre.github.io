---
title: Terms and Definitions
layout: default
parent: Reference
has_toc: true
toc_label: "Test"
toc_sticky: true
toc_levels: 1..3
---

# Terms and Definitions 

## C
---
### Container
A running instance of an image. A Wasm VM instance is created, and the image, along with any configuration, is loaded and then run by the runtime.

### Container image
A template which contains all the necessary elements to run in a container. It is made up of an image manifest and one or more resource files. Once created, an image is immutable, self-describing, and portable. This is sometimes simply referred to as an "image".

### Container set
A collection of one or more container definitions that define what should be running on a given device. This can be thought of as the "application". Each container definition includes:
- A container image
- Configuration values

Values within a container definition are static and cannot be variables or properties. However, these can be overridden when the container set is deployed and, in such cases, can utilize variables and properties.

## D
---
### Device
An endpoint device that is a unit of management. Each device has a unique identifier and is associated with one and only one tenant.

### Device group
A collection of devices. Generally, these share some common characteristics such as location or organizational unit (though that is not strictly required).

## P
---
### Properties
Key/value pairs used to represent data in a simple structure and act much like JSON structures. At present, only string, numeric, and boolean values are supported.

## R
---
### Registry
A hosted service containing repositories of images which responds to the Registry API.

### Repository
A set of images. A repository can be shared by pushing it to a registry server. The different images in the repository can be labeled using tags.

## T
---
### Tenant
A grouping that provides a unit of isolation. Each tenant operates within a segregated environment with its own:
- Devices
- Configuration
- Users
- Access control
- Data
- And more

## V
---
### Variables
Key/value pairs that are defined by the Ocre system. These are generally values for key system attributes such as the current date/time.