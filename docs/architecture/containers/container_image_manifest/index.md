---
layout: default
title: Container Image Manifest 
parent: Containers
nav_order: 1 
---

# Container Image Manifest

The Container Image Manifest is a crucial component of Ocre containers, providing a comprehensive description of the container's structure and contents. It serves as the blueprint that defines and references the *three* main elements of an Ocre container: [*Container Configuration*](../components/container_configuration), a [*WebAssembly Module*](../components/webassembly_module), and (optional) [*Binary Objects*](../components/binary_objects). The manifest ensures that all components can be correctly deployed and executed by the [Ocre Runtime](../../runtime). 

---

### Example

Here's an example of an Ocre Container Image Manifest:

```json
{
  "schemaVersion": 2,
  "mediaType": "application/vnd.oci.image.manifest.v1+json",
  "config": {
    "mediaType": "application/vnd.ocre.image.config.v1+json",
    "digest": "sha256:32b81b11f15ea9c198d59e143b2a5f15446b50b8954ce2e6cfab959988271c78",
    "size": 341
  },
  "layers": [
    {
      "mediaType": "application/vnd.ocre.image.v1.wasm",
      "digest": "sha256:6d61671e8d62f5e77ed054c2d3df4dc9b31e3c74fee7d478f3f2bbdc99a6f256",
      "size": 330
    },
    {
      "mediaType": "application/vnd.ocre.image.v1.blob",
      "digest": "sha256:201ef6570d2967b299cf5ab8f946f061d5db523c106bc90f72729693b798f431",
      "size": 709
    }
  ]
}
```
---

### Configuration Parameters

The elements within the container configuration are defined as follows:

#### Required:
- `schemaVersion` (*int*): Image manifest schema version; per the OCI Image Spec this
MUST be **2**.
- `mediaType` (*string*): Required type of this manifest file. For Ocre containers, this MUST be
**application/vnd.ocre.image.manifest.v1+json**.
- `config` (*object*): References the configuration object for this container image. Schema follows the [OCI Content Descriptor schema](https://github.com/opencontainers/image-spec/blob/main/descriptor.md). It has the
following restrictions:
  - `mediaType` must be set to **application/vnd.ocre.image.config.v1+json**.
- `layers` (*array*): List of elements that comprise this container image. Each layer is a descriptor. One and only one layer MUST be of type **application/vnd.ocre.image.layer.v1.wasm** or **application/vnd.ocre.image.layer.v1.wasm+aot**.

---

### OCI Compliance

Ocre container images follow the [Open Container Initiative (OCI)](https://opencontainers.org/) format where possible, with some modifications to support the needs of constrained, embedded devices. The manifest format is modeled after the [OCI Image Manifest](https://github.com/opencontainers/image-spec/blob/main/manifest.md) format and conforms to the [Wasm OCI Artifact](https://tag-runtime.cncf.io/wgs/wasm/deliverables/wasm-oci-artifact/) layout specifications. This structure ensures that all components of the container are properly defined, versioned, and can be efficiently managed in resource-constrained environments.