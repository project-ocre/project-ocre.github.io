---
layout: default
title: Image Configuration 
parent: Ocre Containers
nav_order: 3
---

# Image Configuration

The container configuration (`application/vnd.wasm.config.v0+json`) defines how the container should be run. It is a collection of metadata that controls the container's runtime behavior, including environment variables, execution parameters, hardware permissions, and entry points.

For Ocre containers, this configuration is specifically tailored to WebAssembly modules running in embedded environments, ensuring proper initialization and execution within resource constraints.

---

## Configuration Structure

The configuration file follows the [OCI Image Manifest](https://github.com/opencontainers/image-spec/blob/main/manifest.md)  specification with modifications for Wasm and embedded environments. See the example and a detailed description of its elements below. 

### Example

```json
{
  "architecture": "wasm",
  "os": "wasip1",
  "layerDigests": [
    "sha256:71cb138990af165c4baf0c43361e5c055ed60a5d2632ee547597be56dcfa07e2"
  ],
  "module": {
    "entryPoint": "on_init"
  }
}
```

## Configuration Elements

Elements within the **configuration** are defined as follows:

| Element | Type | Required | Description |
|---------|------|----------|-------------|
| `architecture` | string | Yes | CPU architecture of the binaries in this image; this value MUST be set to `wasm` |
| `os` | string | Yes | Operating system which this image is built to run on; this MUST be `wasip1` or `wasip2` according to the Wasm target type |
| `layerDigests` | array | Yes | Digests of all of the layers this configuration targets; digests must be in the same order as in the image manifest file |
| `module` | module object | Yes | Specifies the parameters used for execution; `entryPoint` defines the WASM function to call on container start |

---

## Future Enhancements

Ocre will extend its configuration format to better align with OCI standards and support critical runtime elements through the `config` entry. This enhancement will enable control over environment variables, permissions, and additional runtime parameters that are essential for container operation. 

Here's an **example** of how these configurations will be structured:

```json
{
  "config": {
    "Env": [
      "name1=val1",
      "name2=val2"
    ],
    "Permissions": [
      "perm1",
      "perm2"
    ]
  }
}
```