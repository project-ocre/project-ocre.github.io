---
title: Inter-Container Messaging
layout: default
parent: Container
has_toc: false 
---

# Inter-Container Messaging
{: .no_toc }

The Inter-Container Messaging API enables containers to communicate through a publish-subscribe pattern, allowing for decoupled, asynchronous interactions without direct dependencies.

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
#include "messaging.h"
```

---


## Execution Environment

All messaging functions have an internal `wasm_exec_env_t` parameter that is used by the Ocre runtime. This parameter is NOT needed when calling these functions from within a container. Container applications should omit this parameter.

---

## Types

### Message Structure

A structure defining the format of messages used in inter-container communication.

```c
typedef struct ocre_msg {
    uint64_t mid;         ///< Message ID - auto-incremented for each message
    char *topic;          ///< Topic name for the message
    char *content_type;   ///< Content type of the message (MIME type recommended)
    void *payload;        ///< Pointer to the message payload
    int payload_len;      ///< Length of the payload in bytes
} ocre_msg_t;
```

---

## Methods

### Initialize Messaging System

Initializes the inter-container messaging system by setting up a message queue and starting a subscriber thread. If already initialized, the function **logs a warning** but **proceeds without error**.

```c
void ocre_msg_system_init(void);
```

**Returns**:
- None

### Publish Message

Publishes a message to the specified topic.

```c
int ocre_publish_message(char *topic, char *content_type, void *payload, int payload_len);
```

Fails if the messaging system is not initialized, if `topic`, `content_type`, or `payload` is `NULL`/empty, if `payload_len` is `0`, or if the message queue is full.

**Parameters**:

| Name | Type | Description |
| ---- | ---- | ----------- |
| `exec_env` | *wasm_exec_env_t* | Currently unused but required for compatibility with WebAssembly runtime |
| `topic` | *char* | Topic name to publish the message to |
| `content_type` | *char* | Content type of the message (`MIME` type recommended) |
| `payload` | *void* | Pointer to the **message payload buffer** |
| `payload_len` | *int* | Length of the payload in bytes |

**Returns**:

| Value | Description |
| ----- | ----------- |
| `0` | Message published successfully |
| `-1` | Failed to publish message (see error codes) |

### Subscribe to Topic

Subscribes to a topic to receive messages.

```c
int ocre_subscribe_message(char *topic, char *handler_name);
```

**Parameters**:

| Name | Type | Description |
| ---- | ---- | ----------- |
| `exec_env` | *wasm_exec_env_t* | WebAssembly execution environment used to locate the callback function in the WebAssembly module. |
| `topic` | *char* | Topic name to subscribe to |
| `handler_name` | *char* | Name of the callback function to be called when a message is received |

**Returns**:

| Value | Description |
| ----- | ----------- |
| `0` | Subscription successful |
| `-1` | Failed to subscribe (see error codes) |

---

## Error Handling

The Inter-Container Messaging API functions return integer status codes to indicate success or failure. Although all error conditions return the same value (`-1`), different types of errors can occur based on the context and input parameters.

### Error Codes

| Return Value | Description |
|--------------|-------------|
| `0` | Success - The operation completed successfully |
| `-1` | Failure - The operation failed (see error conditions) |

---

Sure — here’s a clean way to combine and tighten what you wrote, keeping the clarity and the important notes:

---

## Examples

The following example demonstrates how to use the Inter-Container Messaging API in Ocre-based containers. Containers communicate asynchronously using a publish-subscribe model, with messages processed in a dedicated subscriber thread managed by the messaging system.

*Two* containers work together:
- The **[Publisher Container](#publisher-container)** sends JSON-formatted temperature readings to the `sensors/temperature` topic every 500 milliseconds.
- The **[Subscriber Container](#subscriber-container)** subscribes to this topic and processes incoming messages.

**Notes**:
- The `wasm_exec_env_t` parameter is required for WebAssembly function signatures but may be unused in some cases (e.g., publishing). In real applications, it is provided by the WASM runtime.
- Use `ocre_sleep` instead of POSIX `sleep`.
- The messaging system automatically handles memory allocation and deallocation for messages passed to callback functions.

---

### Publisher Container

```c
#include <stdio.h>
#include <string.h>
#include "messaging.h"

#define INTERVAL_MS 500

// Initialize the messaging system
static int init_messaging(void) {
    ocre_msg_system_init();
    printf("Messaging system initialized\n");
    return 0;
}

// Publish temperature data
static void publish_temperature(void) {
    char *topic = "sensors/temperature";
    char *content_type = "application/json";
    char payload[64];
    
    static float temp = 22.5;
    snprintf(payload, sizeof(payload), "{\"temperature\": %.1f, \"unit\": \"celsius\"}", temp);
    temp += 0.1;
    int payload_len = strlen(payload) + 1; // Include null terminator

    int ret = ocre_publish_message(NULL, topic, content_type, payload, payload_len);
    if (ret == 0) {
        printf("Published temperature data to %s\n", topic);
    } else {
        printf("Failed to publish temperature data: %d\n", ret);
    }
}

// Application entry point
int main(void) {
    // Set STDIO to line buffering
    setvbuf(stdout, NULL, _IOLBF, 0);

    // Initialize messaging
    if (init_messaging() != 0) {
        printf("Failed to initialize messaging\n");
        return -1;
    }

    // Publish data periodically
    while (1) {
        publish_temperature();
        ocre_sleep(INTERVAL_MS);
    }

    return 0;
}
```

### Subscriber Container

```c

#include <stdio.h>
#include <string.h>
#include "messaging.h"

// Message handler function (exported for WASM)
__attribute__((export_name("temperature_handler")))

void temperature_handler(ocre_msg_t *msg) {
    printf("Received message on topic: %s\n", msg->topic);
    printf("Content type: %s\n", msg->content_type);
    printf("Payload: %s\n", (char *)msg->payload);
    printf("Message ID: %llu\n", msg->mid);

    // In a real application, parse the JSON payload and process the data
}

// Initialize the messaging system and subscribe to topics
static int init_messaging(void) {
    ocre_msg_system_init();
    printf("Messaging system initialized\n");

    int ret = ocre_subscribe_message(NULL, "sensors/temperature", "temperature_handler");
    if (ret != 0) {
        printf("Failed to subscribe: %d\n", ret);
        return ret;
    }
    printf("Subscribed to sensors/temperature\n");
    return 0;
}

// Application entry point
int main(void) {
    // Set STDIO to line buffering
    setvbuf(stdout, NULL, _IOLBF, 0);

    // Initialize messaging and subscription
    if (init_messaging() != 0) {
        printf("Failed to initialize messaging\n");
        return -1;
    }

    // Keep the application running
    while (1) {
        ocre_sleep(100); // Prevent busy waiting
    }

    return 0;
}
```

---

## Reference

| Function | Description | Parameters | Return Value | Error Conditions |
|----------|-------------|------------|--------------|------------------|
| [`ocre_msg_system_init`](#initialize-messaging-system) | Initializes the messaging system | None | None | None |
| [`ocre_publish_message`](#publish-message) | Publishes a message to a topic | `exec_env`: WASM execution environment<br/>`topic`: Topic name<br/>`content_type`: MIME type<br/>`payload`: Message content<br/>`payload_len`: Content length | `0`: Success<br/>`-1`: Failure | Topic is `NULL`/empty<br/>Content type is `NULL`/empty<br/>Payload is `NULL` or length is `0`<br/>Message queue is full |