---
layout: default
title: Image Components 
parent: Ocre Containers 
nav_order: 1 
---

# Image Components

An Ocre container consists of the following **three** components: 

| **Component** | **Description** |
|-----------|-------------|
| [`oci-layout`](#oci-layout) | OCI Layout file in JSON format. Specifies the version of the `imageLayoutVersion` in use. Ocre containers must set this value to `1.0.0` |
| [`index.json`](#indexjson) | Image index file in JSON format. Serves as the entry point for the container image. Ocre containers will have one manifest entry with the mediaType of `application/vnd.oci.image.manifest.v1+json` which points to the Ocre artifact. |
| [`blobs/`](#blobs) | Directory containing content-addressable blobs. Blobs are stored as files with the naming convention of `<alg>/<encoded>` which much match the digest format `<alg>:<encoded>`. Ocre containers support the SHA-256 algorithm *only*. |

### oci-layout

The `oci-layout` file identifies this as an Open Container Image Layout. Written in [UTF-8](https://www.w3schools.com/charsets/ref_html_utf8.asp) encoded JSON, it contains a single field `imageLayoutVersion` which must be set to `1.0.0` for Ocre containers. This file follows the `application/vnd.oci.layout.header.v1+json` media type specification as defined [here](https://github.com/opencontainers/image-spec/blob/v1.1.0-rc2/media-types.md).

An **example** of this file is as follows:
```json
{
    "imageLayoutVersion": "1.0.0"
}
```

### index.json

The `index.json` file serves as the entry point for the container image. Written in [UTF-8](https://www.w3schools.com/charsets/ref_html_utf8.asp) encoded JSON, it contains a single manifest entry pointing to the Ocre artifact, using the `mediaType` of `application/vnd.oci.image.manifest.v1+json`. This file follows the [OCI Image Index Specification](https://github.com/opencontainers/image-spec/blob/v1.1.0-rc2/image-layout.md#indexjson-file).

An **example** of this file is as follows:

```json
{
  "schemaVersion": 2,
  "mediaType": "application/vnd.oci.image.index.v1+json",
  "manifests": [
    {
      "mediaType": "application/vnd.oci.image.manifest.v1+json",
      "digest": "sha256:b11ba766595f3bf6b1db36019cb09decc88aca35ff44dc5ae70bd88d4f188be4",
      "size": 445
    }
  ]
}
```

This illustrates an index that provides a single reference the Ocre container image manifest.

### blobs/

The `blobs` directory contains all content-addressable components of the Ocre container. Files are stored using the format `<alg>/<encoded>`, matching their digest format `<alg>:<encoded>`. Currently, Ocre containers only support the SHA-256 algorithm for these digests.

---

## Learn More
Learn how these components are organized by reading about the [Image Manifest](../image_manifest), which defines the complete structure of an Ocre container.