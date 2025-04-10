---
title: Timers
layout: default
parent: Container APIs
---

# Timers 
{: .no_toc }

The Timers API provides a simple interface for creating and managing timed events in Ocre containers. It supports creating one-shot and periodic timers with customizable intervals. The API leverages the Zephyr kernel's timer functionality to provide accurate timing with millisecond precision.

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
#include "ocre_timer.h"
```

---

## Types 

### Timer Callback Function
A function that will be called when a timer expires. The function receives the timer ID as its parameter.

```c
typedef void (*timer_callback_t)(int timer_id);
```

### Timer Handle
A type that represents a timer instance identifier. 

```c
typedef int ocre_timer_t;
```

Each timer is assigned a unique ID between `1` and `MAX_TIMERS`, which is used to reference the timer in all operations after creation. The timer handle is an opaque identifier that abstracts the underlying timer implementation.

### Execution Environment 

All timer functions have an internal `wasm_exec_env_t` parameter that is used by the Ocre runtime. This parameter is NOT needed when calling these functions from within a container. Container applications should omit this parameter.

---

## Methods 

### Initializing the Timer System

Sets the current WASM module instance for timer callbacks. Must be called before creating any timers.

```c
void ocre_timer_set_module_inst(wasm_module_inst_t module_inst);
```

### Create Timer

Creates a new timer with the specified ID (1-5).

```c
int ocre_timer_create(int id);
```

**Parameters**:

| Name | Type | Description |
| ------ | ------ | ----------- |
| `id` | *int* | Timer identifier (must be between `1` and `MAX_TIMERS`) |

**Returns**:

| Value | Description |
| ------ | ----------- |
| `0` | on success |
| `-1` | on error with errno set |

**Errors**: 

| Value | Description |
| ------ | ----------- |
| `EINVAL` | Invalid timer ID or timer system not initialized |
| `EEXIST` | Timer ID already in use |

### Start Timer

Starts the specified timer with the given interval.

```c
int ocre_timer_start(ocre_timer_t id, int interval, int is_periodic);
```

**Parameters**:

| Name | Type | Description |
| ------ | ------ | ----------- |
| `id` | *ocre_timer_t* | Timer identifier |
| `interval` | *int* | Timer interval in milliseconds |
| `is_periodic` | *int* | `1` for periodic timer, `0` for one-shot |

**Returns**:

| Value | Description |
| ------ | ----------- |
| `0` | on success |
| `-1` | on error with errno set |

**Errors**:

| Value | Description |
| ------ | ----------- |
| `EINVAL` | Invalid timer ID, timer not found, or invalid interval |

### Stop Timer

Stops the specified timer.

```c
int ocre_timer_stop(ocre_timer_t id);
```

**Parameters**:

| Name | Type | Description |
| ------ | ------ | ----------- |
| `id` | *ocre_timer_t* | Timer identifier |

**Returns**:

| Value | Description |
| ------ | ----------- |
| `0` | on success |
| `-1` | on error with errno set |

**Errors**:

| Value | Description |
| ------ | ----------- |
| `EINVAL` | Invalid timer ID, timer not found, or invalid interval |

### Get Remaining Time

Retrieves the remaining time for a timer.

```c
int ocre_timer_get_remaining(ocre_timer_t id);
```

**Parameters**:

| Name | Type | Description |
| ------ | ------ | ----------- |
| `id` | *ocre_timer_t* | Timer identifier |

**Returns**:

| Value | Description |
| ------ | ----------- |
| `>= 0` | Remaining time in milliseconds |
| `-1` | Error occurred (check errno) |

**Errors**:

| Value | Description |
| ------ | ----------- |
| `EINVAL` | Invalid timer ID, timer not found, or invalid interval |

### Delete Timer

Stops and deletes the specified timer.

```c
int ocre_timer_delete(ocre_timer_t id);
```

**Parameters**:

| Name | Type | Description |
| ------ | ------ | ----------- |
| `id` | *ocre_timer_t* | Timer identifier |

**Returns**:

| Value | Description |
| ------ | ----------- |
| `0` | on success |
| `-1` | on error with errno set |

**Errors**:

| Value | Description |
| ------ | ----------- |
| `EINVAL` | Invalid timer ID, timer not found, or invalid interval |


### Setting the Dispatcher

Sets the WASM dispatcher function for timer callbacks. This function is used internally by the Ocre runtime to look up the `timer_callback` function in your module. Container applications do not need to call this function directly.

```c
void ocre_timer_set_dispatcher(void);
```

{: .note }
This function is handled automatically by the Ocre runtime and is included here for completeness. Container applications should not call this function directly.

---
## Error Handling

All functions, except `ocre_timer_set_module_inst` and `ocre_timer_set_dispatcher`, return `0` on success and `-1` on error. When an error occurs, errno is set to indicate the specific error.

**Errors:**

| Value | Description |
| ------ | ----------- |
| `EINVAL` | Invalid timer ID or timer system not initialized |
| `EEXIST` | Timer ID already in use |

---

## Examples

### Creating and Using a One-shot Timer

```c
#include <stdio.h>
#include "ocre_timer.h"

// Timer callback function
void timer_callback(int timer_id) {
    printf("Timer %d expired!\n", timer_id);
}

// Export the callback with the name the runtime is looking for
__attribute__((export_name("timer_callback"))) void exported_timer_callback(int timer_id) {
    timer_callback(timer_id);
}

int main() {
    // Create timer with ID 1
    const int timer_id = 1;
    
    if (ocre_timer_create(timer_id) != 0) {
        printf("Failed to create timer: %s\n", strerror(errno));
        return -1;
    }
    
    // Start one-shot timer with 1000ms (1 second) interval
    if (ocre_timer_start(timer_id, 1000, 0) != 0) {
        printf("Failed to start timer: %s\n", strerror(errno));
        return -1;
    }
    
    // The timer will call timer_callback when it expires
    // The Ocre runtime handles the main event loop
    return 0;
}
```

### Creating a Periodic Timer

```c
#include <stdio.h>
#include "ocre_timer.h"

// Timer callback function
void timer_callback(int timer_id) {
    printf("Timer tick from timer %d\n", timer_id);
    // Update application state here
}

// Export the callback for the runtime
__attribute__((export_name("timer_callback"))) void exported_timer_callback(int timer_id) {
    timer_callback(timer_id);
}

int main() {
    // Create timer with ID 2
    const int timer_id = 2;
    const int interval_ms = 500;
    const int is_periodic = 1;
    
    // Create the timer
    if (ocre_timer_create(timer_id) != 0) {
        printf("Failed to create timer\n");
        return 1;
    } 
    
    // Start periodic timer with 500ms interval
    if (ocre_timer_start(timer_id, interval_ms, is_periodic) != 0) {
        printf("Failed to start timer\n");
        ocre_timer_delete(timer_id);
        return 1;
    }
    
    printf("Periodic timer started successfully\n");
    // The Ocre runtime will handle the main loop and timer callbacks
    return 0;
}
```

---

## Reference

| Function | Description | Parameters | Return Value | Errors |
|----------|-------------|------------|--------------|--------|
| [`ocre_timer_set_module_inst`](#initializing-the-timer-system) | Sets current WASM module | `module_inst`: WASM module instance | None | None |
| [`ocre_timer_create`](#create-timer) | Creates a timer | `id`: Timer ID (1-5) | `0` on success, `-1` on error | `EINVAL`, `EEXIST` |
| [`ocre_timer_start`](#start-timer) | Starts a timer | `id`: Timer ID<br/>`interval`: Time in ms<br/>`is_periodic`: Boolean flag | `0` on success, `-1` on error | `EINVAL` |
| [`ocre_timer_stop`](#stop-timer) | Stops a timer | `id`: Timer ID | `0` on success, `-1` on error | `EINVAL` |
| [`ocre_timer_get_remaining`](#get-remaining-time) | Gets remaining time | `id`: Timer ID | Remaining ms or `-1` on error | `EINVAL` |
| [`ocre_timer_delete`](#delete-timer) | Deletes a timer | `id`: Timer ID | `0` on success, `-1` on error | `EINVAL` |
| [`ocre_timer_set_dispatcher`](#setting-the-dispatcher) | Sets callback dispatcher | None | None | None |