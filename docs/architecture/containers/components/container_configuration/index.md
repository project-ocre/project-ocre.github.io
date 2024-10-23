---
layout: default
title: Container Configuration 
parent: Components
nav_order: 1
---


The container configuration defines how a container should be run on the device. It provides essential metadata that dictates the behavior of the container, including parameters like environment variables, entry points, and permissions. These configurations ensure that containers operate within defined constraints and have access only to necessary system resources.

---

### Example:
```json
{
  "architecture": "amd64",
  "os": "nubix",
  "config": {
    "Entrypoint": "on_init",
    "Environment": [
      "VAR1=value1",
      "VAR2=value2"
    ],
    "Permissions": [
      "filesystem",
      "http",
      "i2c",
      "gpio"
    ]
  }
}
```
### Configuration Parameters

The elements within the container configuration are defined as follows:

#### Required:
- `architecture` (*string*): CPU architecture of the binaries in this image; this value MUST be set to **wasm**. This parameter also supports an optional variant value. For example, `thumbv4`.
- `os` (*string*): Operating system which this image is built to run on; this MUST be **nubix**.
- `config` (*object*): Specifies the parameters used for execution
  - `EntryPoint`: The WASM function to call on container start.
  - `Environment`: Array of variables to set in the container environment.
  - `Permissions`: Aarray of permissions granted to the container.

#### Optional:
- `os.version`: Specify version of the operating system.
- `os.features` Specify os-specific features.