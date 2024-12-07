---
layout: default
title: Image Format 
parent: Ocre Containers 
nav_order: 0 
---

# Image Format

Ocre container images follow the [Open Container Initiative (OCI)](https://opencontainers.org/) format where possible, with some modifications to support the needs of constrained, embedded devices. The manifest format is modeled after the [OCI Image Manifest](https://github.com/opencontainers/image-spec/blob/main/manifest.md) format and conforms to the [Wasm OCI Artifact](https://tag-runtime.cncf.io/wgs/wasm/deliverables/wasm-oci-artifact/) layout specifications.

Ocre containers can be stored either as a single zip file or as a set of files in a directory, providing flexibility in how they are distributed and managed. The container format uses the Wasm OCI Artifact layout as its `config.mediaType`, ensuring compatibility with standard OCI tooling while maintaining the specific requirements for WebAssembly modules in embedded environments.


---

## Learn More

To fully understand the Ocre container format, explore these detailed specifications. 

- [Image Components](../image_components): Learn about the core elements that make up an Ocre container. 
- [Image Manifest](../image_manifest): Understand how the manifest file organizes and references container components.
- [Image Configuration](../image_configuration): Discover how to configure container execution, permissions, and runtime behavior.