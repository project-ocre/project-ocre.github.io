---
layout: default
title: WebAssembly Modules 
parent: Components
nav_order: 0
---

# WebAssembly Modules

Ocre containers use [WebAssembly (WASM)](https://webassembly.org/) modules as the format for executable code. These modules are stateless binaries compiled to run on the WASM virtual machine.

Modules can either be pure WebAssembly or Ahead-of-Time (AOT) compiled binaries optimized for a specific platform target. In general, the Ocre Orchestration Hub will automatically AOT compile modules on-demand based on the architecture of the deployment target. However, there are instances where a pure WASM binary may be preferred.

{: .note}
Currently, only a single WASM module is supported per container image. However, we are exploring extending this to support multiple modules and/or components via the WebAssembly Component Model.