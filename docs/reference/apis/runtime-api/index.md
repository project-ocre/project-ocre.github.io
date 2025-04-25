---
title: Runtime
layout: default
parent: APIs
has_toc: false 
---

# Runtime API
{: .no_toc }

The Ocre Runtime API enables full container lifecycle management and can be integrated into various environments including real-time operating systems ([Zephyr](https://zephyrproject.org/), [FreeRTOS](https://www.freertos.org/)), Linux-based platforms (including [Yocto](https://www.yoctoproject.org/)), or custom firmware implementations. With these interfaces, platform developers can initialize the runtime, deploy containers, monitor their execution, and control their operation throughout the system lifecycle.

---

## Table of Contents
{: .no_toc }

Navigate this comprehensive API reference using the links below.

<details close markdown="block">
  <summary>
  Click to expand 
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---

## Header File

```c
#include "ocre_container_runtime.h"
```

---

## Types

### Runtime Status

An enumeration defining the possible states of the container runtime.

```c
typedef enum {
    RUNTIME_STATUS_UNKNOWN,     ///< Status is unknown.
    RUNTIME_STATUS_INITIALIZED, ///< Runtime has been initialized.
    RUNTIME_STATUS_DESTROYED,   ///< Runtime has been destroyed.
    RUNTIME_STATUS_ERROR        ///< An error occurred with the runtime.
} ocre_container_runtime_status_t;
```

### Container Status

An enumeration defining the possible states of a container.

```c
typedef enum {
    CONTAINER_STATUS_UNKNOWN,      ///< Status is unknown.
    CONTAINER_STATUS_CREATED,      ///< Container has been created.
    CONTAINER_STATUS_RUNNING,      ///< Container is currently running.
    CONTAINER_STATUS_STOPPED,      ///< Container has been stopped.
    CONTAINER_STATUS_DESTROYED,    ///< Container has been destroyed.
    CONTAINER_STATUS_UNRESPONSIVE, ///< Container is unresponsive (used with health checks).
    CONTAINER_STATUS_ERROR,        ///< An error occurred with the container.
} ocre_container_status_t;
```

### Container Permissions

An enumeration defining the permission types for containers.

```c
typedef enum {
    OCRE_CONTAINER_PERM_READ_ONLY,  ///< Container has read-only permissions.
    OCRE_CONTAINER_PERM_READ_WRITE, ///< Container has read and write permissions.
    OCRE_CONTAINER_PERM_EXECUTE     ///< Container has execute permissions.
} ocre_container_permissions_t;
```

{: .note }
Not currently used.

### Runtime Arguments

A structure containing the runtime arguments for a container runtime.

```c
typedef struct ocre_runtime_arguments_t {
    uint32_t size;                  ///< Size of the buffer.
    char *buffer;                   ///< Pointer to the buffer containing the WASM module.
    char error_buf[128];            ///< Buffer to store error messages.
    wasm_module_t module;           ///< Handle to the loaded WASM module.
    wasm_module_inst_t module_inst; ///< Handle to the instantiated WASM module.
    wasm_function_inst_t func;      ///< Handle to the function to be executed within the WASM module.
    wasm_exec_env_t exec_env;       ///< Execution environment for the WASM function.
    uint32_t stack_size;            ///< Stack size for the WASM module.
    uint32_t heap_size;             ///< Heap size for the WASM module.
} ocre_runtime_arguments_t;
```

### Container Data

A structure representing the data associated with a container. All fields must be initialized to avoid undefined behavior.

```c
typedef struct ocre_container_data_t {
    char name[16];        ///< Name of the module (must be unique per installed instance).
    char sha256[70];      ///< SHA-256 hash of the WASM file, used to construct the file path (e.g., /lfs/ocre/images/<sha256>.bin).
    uint32_t stack_size;  ///< Stack size for the WASM module.
    uint32_t heap_size;   ///< Heap size for the WASM module.
    ocre_healthcheck WDT; ///< Watchdog timer for container health checking (set to NULL if unused).
    int watchdog_interval;///< Watchdog interval in milliseconds (set to 0 if unused).
    int timers;           ///< Number of timers allocated for the container (set to 0 if unused).
} ocre_container_data_t;
```

### Container Init Arguments

A structure with initialization arguments for the container runtime.

```c
typedef struct ocre_container_runtime_init_arguments_t {
    uint32_t default_stack_size; ///< Default stack size for WASM modules.
    uint32_t default_heap_size;  ///< Default heap size for WASM modules.
    int maximum_containers;      ///< Maximum number of containers allowed (must not exceed MAX_CONTAINERS).
    NativeSymbol *ocre_api_functions; ///< Pointer to an array of host functions for WASM modules (set to NULL for defaults).
} ocre_container_init_arguments_t;
```

### Container

A structure representing a container in the runtime.

```c
typedef struct ocre_container_t {
    ocre_runtime_arguments_t ocre_runtime_arguments;  ///< Runtime arguments for the container.
    ocre_container_status_t container_runtime_status; ///< Current status of the container.
    ocre_container_data_t ocre_container_data;        ///< Container-specific data.
} ocre_container_t;
```

### Container Runtime Context

An opaque structure used internally by the runtime. Do not modify directly.

```c
typedef struct ocre_cs_ctx {
    ocre_container_t containers[MAX_CONTAINERS];
    int current_container_id;
    int download_count;
} ocre_cs_ctx;
```

### Container Runtime Callback

A function prototype for container runtime callbacks, invoked when a container operation completes (e.g., creation, running).

```c
typedef void (*ocre_container_runtime_cb)(void);
```

---

## Methods

### Runtime Initialization

Initializes the container runtime environment. Blocks until the runtime is initialized or an error occurs.

```c
ocre_container_runtime_status_t ocre_container_runtime_init(ocre_cs_ctx *ctx, ocre_container_init_arguments_t *args);
```

**Parameters**:

| Name | Type | Description |
| ------ | ------ | ----------- |
| `ctx` | *ocre_cs_ctx* * | Pointer to the container runtime context structure |
| `args` | *ocre_container_init_arguments_t* * | Pointer to the runtime arguments structure, with initialized fields |

**Returns**:

| Value | Description |
| ------ | ----------- |
| `RUNTIME_STATUS_INITIALIZED` | Runtime successfully initialized |
| `RUNTIME_STATUS_ERROR` | Failed to initialize runtime |

### Runtime Destruction

Destroys the container runtime environment. Blocks until the runtime is destroyed.

```c
ocre_container_status_t ocre_container_runtime_destroy(void);
```

**Returns**:

| Value | Description |
| ------ | ----------- |
| `RUNTIME_STATUS_DESTROYED` | Runtime successfully destroyed |

### Create Container

Creates a new container within the runtime, loading the WASM module specified by `container_data.sha256`.

```c
ocre_container_status_t ocre_container_runtime_create_container(ocre_cs_ctx *ctx,
                                                                struct ocre_container_data_t *container_data,
                                                                int *container_id, ocre_container_runtime_cb callback);
```

**Parameters**:

| Name | Type | Description |
| ------ | ------ | ----------- |
| `ctx` | *ocre_cs_ctx* * | Pointer to the container runtime context structure |
| `container_data` | *ocre_container_data_t* * | Pointer to the container data structure, with all fields initialized |
| `container_id` | *int* * | Pointer to receive the assigned container `ID` |
| `callback` | *ocre_container_runtime_cb* | Optional callback function invoked when the container is created (set to `NULL` if unused) |

**Returns**:

| Value | Description |
| ------ | ----------- |
| `CONTAINER_STATUS_CREATED` | Container successfully created |
| `CONTAINER_STATUS_ERROR` | Failed to create container (e.g., no available slots, invalid WASM module) |

### Run Container

Executes a specific container.

```c
ocre_container_status_t ocre_container_runtime_run_container(ocre_cs_ctx *ctx, int container_id,
                                                             ocre_container_runtime_cb callback);
```

**Parameters**:

| Name | Type | Description |
| ------ | ------ | ----------- |
| `ctx` | *ocre_cs_ctx* * | Pointer to the container runtime context structure |
| `container_id` | *int* | The container `ID` to run |
| `callback` | *ocre_container_runtime_cb* | Optional callback function invoked when the container starts (set to `NULL` if unused) |

**Returns**:

| Value | Description |
| ------ | ----------- |
| `CONTAINER_STATUS_RUNNING` | Container successfully started |
| `CONTAINER_STATUS_ERROR` | Failed to run container (e.g., invalid container `ID`) |

### Get Container Status

Retrieves the current status of a specific container.

```c
ocre_container_status_t ocre_container_runtime_get_container_status(ocre_cs_ctx *ctx, int container_id);
```

**Parameters**:

| Name | Type | Description |
| ------ | ------ | ----------- |
| `ctx` | *ocre_cs_ctx* * | Pointer to the container runtime context structure |
| `container_id` | *int* | The container `ID` to check |

**Returns**:

| Value | Description |
| ------ | ----------- |
| The current status of the specified container | Success |
| `CONTAINER_STATUS_ERROR` | Invalid container `ID` |

### Stop Container

Stops a running container.

```c
ocre_container_status_t ocre_container_runtime_stop_container(ocre_cs_ctx *ctx, int container_id,
                                                              ocre_container_runtime_cb callback);
```

**Parameters**:

| Name | Type | Description |
| ------ | ------ | ----------- |
| `ctx` | *ocre_cs_ctx* * | Pointer to the container runtime context structure |
| `container_id` | *int* | The container `ID` to stop |
| `callback` | *ocre_container_runtime_cb* | Optional callback function invoked when the container stops (set to `NULL` if unused) |

**Returns**:

| Value | Description |
| ------ | ----------- |
| `CONTAINER_STATUS_STOPPED` | Container successfully stopped |
| `CONTAINER_STATUS_ERROR` | Failed to stop container |

### Destroy Container

Destroys and unloads a container from the runtime.

```c
ocre_container_status_t ocre_container_runtime_destroy_container(ocre_cs_ctx *ctx, int container_id,
                                                                 ocre_container_runtime_cb callback);
```

**Parameters**:

| Name | Type | Description |
| ------ | ------ | ----------- |
| `ctx` | *ocre_cs_ctx* * | Pointer to the container runtime context structure |
| `container_id` | *int* | The container `ID` to destroy |
| `callback` | *ocre_container_runtime_cb* | Optional callback function invoked when the container is destroyed (set to `NULL` if unused) |

**Returns**:

| Value | Description |
| ------ | ----------- |
| `CONTAINER_STATUS_DESTROYED` | Container successfully destroyed |
| `CONTAINER_STATUS_ERROR` | Failed to destroy container |

### Restart Container

Restarts a running container.

```c
ocre_container_status_t ocre_container_runtime_restart_container(ocre_cs_ctx *ctx, int container_id,
                                                                 ocre_container_runtime_cb callback);
```

**Parameters**:

| Name | Type | Description |
| ------ | ------ | ----------- |
| `ctx` | *ocre_cs_ctx* * | Pointer to the container runtime context structure |
| `container_id` | *int* | The container `ID` to restart |
| `callback` | *ocre_container_runtime_cb* | Optional callback function invoked when the container restarts (set to `NULL` if unused) |

**Returns**:

| Value | Description |
| ------ | ----------- |
| Current status of the container | Success (typically `CONTAINER_STATUS_RUNNING`) |
| `CONTAINER_STATUS_ERROR` | Failed to restart container |

---

## Error Handling

The Runtime API functions return status codes that indicate the success or failure of operations. These codes, defined in `ocre_container_runtime_status_t` and `ocre_container_status_t`, allow developers to monitor the state of the runtime and containers. The following table summarizes the key status codes and their meanings:

| Value | Description |
|----------|-------------|
| `RUNTIME_STATUS_INITIALIZED` | Runtime was successfully initialized |
| `RUNTIME_STATUS_ERROR` | An error occurred in the runtime (e.g., initialization failure) |
| `RUNTIME_STATUS_DESTROYED` | Runtime was successfully destroyed |
| `CONTAINER_STATUS_CREATED` | Container was successfully created |
| `CONTAINER_STATUS_RUNNING` | Container is running |
| `CONTAINER_STATUS_STOPPED` | Container was successfully stopped |
| `CONTAINER_STATUS_DESTROYED` | Container was successfully destroyed |
| `CONTAINER_STATUS_ERROR` | Invalid container `ID`, invalid WASM module, or other error |

To handle these status codes effectively, check the return value of each API call and take appropriate action based on the result. For example, when querying a container's status, use a switch statement to interpret the status and respond accordingly:

```c
// Check the container status
ocre_container_status_t status = ocre_container_runtime_get_container_status(&ctx, container_id);
switch (status) {
    case CONTAINER_STATUS_RUNNING:
        printf("Container is running\n");
        break;
    case CONTAINER_STATUS_STOPPED:
        printf("Container is stopped\n");
        break;
    case CONTAINER_STATUS_ERROR:
        printf("Container is in an error state\n");
        break;
    default:
        printf("Container is in state: %d\n", status);
        break;
}
```

---

## Examples

### Initializing the Runtime

This example demonstrates initializing the Ocre runtime with specified stack and heap sizes.

```c
#include <zephyr/kernel.h>
#include <ocre/ocre_container_runtime.h>

int main() {
    ocre_cs_ctx ctx;
    ocre_container_init_arguments_t args = {0}; // Zero-initialize to avoid undefined behavior

    // Set up initialization arguments
    args.default_stack_size = 8192; // 8 KB stack size
    args.default_heap_size = 16384; // 16 KB heap size
    args.maximum_containers = 5;     // Allow up to 5 containers
    args.ocre_api_functions = NULL;  // Use default WebAssembly host functions

    // Initialize the Ocre runtime
    ocre_container_runtime_status_t ret = ocre_container_runtime_init(&ctx, &args);
    
    if (ret == RUNTIME_STATUS_INITIALIZED) {
        printf("Ocre runtime initialized successfully\n");
        // Continue with container creation and operation...
    } else {
        printf("Failed to initialize runtime: %d\n", ret);
        return -1;
    }
    
    // Clean up
    ocre_container_runtime_destroy();
    return 0;
}
```

### Creating and Running a Container

This example creates a container from a (minimal valid) WebAssembly module, runs it, and performs cleanup. It includes error handling and proper initialization.

```c
#include <zephyr/kernel.h>
#include <zephyr/fs/fs.h>
#include <zephyr/logging/log.h>
#include <ocre/ocre.h>
#include <ocre/fs/fs.h>
#include <ocre/ocre_container_runtime/ocre_container_runtime.h>

LOG_MODULE_REGISTER(ocre_example, LOG_LEVEL_INF);

int create_sample_container(char *file_name) {
    struct fs_file_t f;
    char file_path[64];
    int res;

    snprintf(file_path, sizeof(file_path), "/lfs/ocre/images/%s.bin", file_name);

    fs_file_t_init(&f);
    res = fs_open(&f, file_path, FS_O_CREATE | FS_O_RDWR);
    if (res < 0) {
        LOG_ERR("Failed to open file %s: %d", file_path, res);
        return res;
    }

    // Minimal valid WASM module (no-op, 45 bytes)
    unsigned char wasm_binary[] = {
        0x00, 0x61, 0x73, 0x6D, 0x01, 0x00, 0x00, 0x00, // WASM header
        0x01, 0x04, 0x01, 0x60, 0x00, 0x00, // Type section: () -> ()
        0x03, 0x02, 0x01, 0x00, // Function section: one function
        0x07, 0x07, 0x01, 0x03, 0x72, 0x75, 0x6E, 0x00, 0x00, // Export "run"
        0x0A, 0x04, 0x01, 0x02, 0x00, 0x0B // Code section: empty function
    };

    res = fs_write(&f, wasm_binary, sizeof(wasm_binary));
    if (res != sizeof(wasm_binary)) {
        LOG_ERR("Failed to write WASM binary: %d", res);
        fs_close(&f);
        return -EIO;
    }

    res = fs_close(&f);
    if (res < 0) {
        LOG_ERR("Failed to close file %s: %d", file_path, res);
        return res;
    }

    return 0;
}

int main(int argc, char *argv[]) {
    ocre_cs_ctx ctx;
    ocre_container_init_arguments_t args = {0};
    char *container_filename = "hello";
    int ret;

    // Initialize storage
    ret = ocre_app_storage_init();
    if (ret < 0) {
        LOG_ERR("Failed to initialize storage: %d", ret);
        return ret;
    }

    // Create WASM container image
    ret = create_sample_container(container_filename);
    if (ret < 0) {
        LOG_ERR("Failed to create container image");
        return ret;
    }

    // Initialize runtime
    args.default_stack_size = 8192;
    args.default_heap_size = 16384;
    args.maximum_containers = 5;
    args.ocre_api_functions = NULL;

    ocre_container_runtime_status_t status = ocre_container_runtime_init(&ctx, &args);
    if (status != RUNTIME_STATUS_INITIALIZED) {
        LOG_ERR("Ocre runtime failed to start: %d", status);
        return -1;
    }

    // Create container
    ocre_container_data_t container_data = {0};
    int container_id;
    ocre_container_runtime_cb callback = NULL;

    container_data.heap_size = 16384;
    container_data.stack_size = 8192;
    snprintf(container_data.name, sizeof(container_data.name), "Hello World");
    // Note: In production, compute SHA-256 hash of the WASM file
    snprintf(container_data.sha256, sizeof(container_data.sha256), "%s", container_filename);
    container_data.timers = 0;
    container_data.watchdog_interval = 0;
    container_data.WDT = NULL;

    status = ocre_container_runtime_create_container(&ctx, &container_data, &container_id, callback);
    if (status != CONTAINER_STATUS_CREATED) {
        LOG_ERR("Failed to create container: %d", status);
        ocre_container_runtime_destroy();
        return -1;
    }

    // Run container
    status = ocre_container_runtime_run_container(&ctx, container_id, callback);
    if (status != CONTAINER_STATUS_RUNNING) {
        LOG_ERR("Failed to run container: %d", status);
        ocre_container_runtime_destroy_container(&ctx, container_id, NULL);
        ocre_container_runtime_destroy();
        return -1;
    }

    // Monitor for 10 seconds
    for (int i = 0; i < 10; i++) {
        status = ocre_container_runtime_get_container_status(&ctx, container_id);
        LOG_INF("Container status: %d", status);
        if (status == CONTAINER_STATUS_ERROR) {
            break;
        }
        k_msleep(1000);
    }

    // Clean up
    status = ocre_container_runtime_stop_container(&ctx, container_id, NULL);
    if (status != CONTAINER_STATUS_STOPPED) {
        LOG_ERR("Failed to stop container: %d", status);
    }

    status = ocre_container_runtime_destroy_container(&ctx, container_id, NULL);
    if (status != CONTAINER_STATUS_DESTROYED) {
        LOG_ERR("Failed to destroy container: %d", status);
    }

    ocre_container_runtime_destroy();
    return 0;
}
```

### Container Lifecycle Management

This example demonstrates the full container lifecycle, including creation, running, stopping, and destruction.

```c
#include <zephyr/kernel.h>
#include <zephyr/logging/log.h>
#include <ocre/ocre_container_runtime.h>

LOG_MODULE_REGISTER(ocre_lifecycle, LOG_LEVEL_INF);

int main() {
    ocre_cs_ctx ctx;
    ocre_container_init_arguments_t args = {0};

    // Initialize runtime
    args.default_stack_size = 8192;
    args.default_heap_size = 16384;
    args.maximum_containers = 5;
    args.ocre_api_functions = NULL;

    ocre_container_runtime_status_t ret = ocre_container_runtime_init(&ctx, &args);
    if (ret != RUNTIME_STATUS_INITIALIZED) {
        LOG_ERR("Failed to initialize runtime: %d", ret);
        return -1;
    }

    // Create container
    ocre_container_data_t container_data = {0};
    int container_id;
    container_data.heap_size = 16384;
    container_data.stack_size = 8192;
    snprintf(container_data.name, sizeof(container_data.name), "Sample");
    snprintf(container_data.sha256, sizeof(container_data.sha256), "sample");
    container_data.timers = 0;
    container_data.watchdog_interval = 0;
    container_data.WDT = NULL;

    ocre_container_status_t status = ocre_container_runtime_create_container(
        &ctx, &container_data, &container_id, NULL);
    if (status != CONTAINER_STATUS_CREATED) {
        LOG_ERR("Failed to create container: %d", status);
        ocre_container_runtime_destroy();
        return -1;
    }

    // Run container
    status = ocre_container_runtime_run_container(&ctx, container_id, NULL);
    if (status != CONTAINER_STATUS_RUNNING) {
        LOG_ERR("Failed to run container: %d", status);
        ocre_container_runtime_destroy_container(&ctx, container_id, NULL);
        ocre_container_runtime_destroy();
        return -1;
    }

    // Wait and check status
    k_msleep(5000);
    status = ocre_container_runtime_get_container_status(&ctx, container_id);
    LOG_INF("Container status after 5 seconds: %d", status);

    // Stop container
    status = ocre_container_runtime_stop_container(&ctx, container_id, NULL);
    if (status != CONTAINER_STATUS_STOPPED) {
        LOG_ERR("Failed to stop container: %d", status);
    }

    // Destroy container
    status = ocre_container_runtime_destroy_container(&ctx, container_id, NULL);
    if (status != CONTAINER_STATUS_DESTROYED) {
        LOG_ERR("Failed to destroy container: %d", status);
    }

    // Shutdown runtime
    ocre_container_runtime_destroy();
    return 0;
}
```

---

## Reference

| Function | Description | Parameters | Return Value | Error Codes |
|----------|-------------|------------|--------------|--------------|
| [`ocre_container_runtime_init`](#runtime-initialization) | Initializes the runtime | `ctx`: Runtime context<br/>`args`: Init arguments | Runtime status | `RUNTIME_STATUS_ERROR` |
| [`ocre_container_runtime_destroy`](#runtime-destruction) | Destroys the runtime | None | Runtime status | None |
| [`ocre_container_runtime_create_container`](#create-container) | Creates a container | `ctx`: Runtime context<br/>`container_data`: Container config<br/>`container_id`: Receives `ID`<br/>`callback`: Optional callback | Container status | `CONTAINER_STATUS_ERROR` |
| [`ocre_container_runtime_run_container`](#run-container) | Runs a container | `ctx`: Runtime context<br/>`container_id`: Container `ID`<br/>`callback`: Optional callback | Container status | `CONTAINER_STATUS_ERROR` |
| [`ocre_container_runtime_get_container_status`](#get-container-status) | Gets container status | `ctx`: Runtime context<br/>`container_id`: Container `ID` | Container status | `CONTAINER_STATUS_ERROR` |
| [`ocre_container_runtime_stop_container`](#stop-container) | Stops a container | `ctx`: Runtime context<br/>`container_id`: Container `ID`<br/>`callback`: Optional callback | Container status | `CONTAINER_STATUS_ERROR` |
| [`ocre_container_runtime_destroy_container`](#destroy-container) | Destroys a container | `ctx`: Runtime context<br/>`container_id`: Container `ID`<br/>`callback`: Optional callback | Container status | `CONTAINER_STATUS_ERROR` |
| [`ocre_container_runtime_restart_container`](#restart-container) | Restarts a container | `ctx`: Runtime context<br/>`container_id`: Container `ID`<br/>`callback`: Optional callback | Container status | `CONTAINER_STATUS_ERROR` |

---

## Notes

- **WASM Module Requirements**: The WebAssembly module specified by `container_data.sha256` must be a valid, complete binary stored in the filesystem (e.g., `/lfs/ocre/images/<sha256>.bin`). Invalid or truncated modules will cause `CONTAINER_STATUS_ERROR`.
- **SHA-256 Usage**: The sha256 field in `ocre_container_data_t` must contain the SHA-256 hash of the WASM file as a **hexadecimal string**. Using a filename instead of a hash is not recommended and may cause file loading errors unless explicitly supported by the runtime.
- **Host Functions**: The `ocre_api_functions` field in `ocre_container_init_arguments_t` allows defining custom WebAssembly host functions. Set to `NULL` to use default functions provided by the runtime.
- **Error Handling**: Always check return values of API calls, as shown in the examples, to handle errors gracefully and prevent resource leaks.
- **Resource Cleanup**: Ensure proper cleanup by calling `ocre_container_runtime_stop_container`, `ocre_container_runtime_destroy_container`, and `ocre_container_runtime_destroy` when operations are complete.