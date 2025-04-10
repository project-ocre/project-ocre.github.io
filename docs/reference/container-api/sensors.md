---
title: Sensors
layout: default
parent: Container APIs
---

# Sensors
{: .no_toc }

The Sensors API provides a unified interface for discovering, configuring, and retrieving data from various hardware sensors in Ocre containers. It supports multiple sensor types and channels, allowing applications to interact with environmental, motion, and position sensing capabilities.

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
#include "ocre_sensors.h"
```

---

## Types

### Sensor Callback Function

A function that will be called when a sensor trigger event occurs.

```c
typedef void (*ocre_sensor_trigger_cb)(ocre_sensor_handle_t sensor_handle, sensor_channel_t channel,
                                       ocre_sensor_value_t *data, void *ptr);
```

### Sensor Handle

A type that represents a sensor device identifier.

```c
typedef struct ocre_sensor_handle_t {
    int id;
    const char *device_name;
    struct device sensor_device;
} ocre_sensor_handle_t;
```

The sensor handle contains a unique ID, device name, and a reference to the underlying device structure. It is used to reference a specific sensor in all operations.

### Sensor Instance

A structure representing a sensor instance with its channels.

```c
typedef struct ocre_sensor_t {
    ocre_sensor_handle_t handle; ///< Sensor handle
    int num_channels;            ///< Number of supported channels
    sensor_channel_t channels[]; ///< Supported channels
} ocre_sensor_t;
```

### Sensor Value

A structure representing a sensor value with integer and fractional components.

```c
typedef struct ocre_sensor_value_t {
    int32_t integer;
    int32_t floating;
} ocre_sensor_value_t;
```

### Sensor Sample

A structure representing a sample read from a sensor.

```c
typedef struct ocre_sensors_sample_t {
    ocre_sensor_value_t value;
} ocre_sensors_sample_t;
```

### Sensor Channels

An enumeration of the different types of sensor channels available.

```c
typedef enum {
    SENSOR_CHANNEL_ACCELERATION = 0x0C,
    SENSOR_CHANNEL_GYRO = 0x50,
    SENSOR_CHANNEL_MAGNETIC_FIELD = 0x32,
    SENSOR_CHANNEL_LIGHT = 0x0F,
    SENSOR_CHANNEL_PRESSURE = 0x7A,
    SENSOR_CHANNEL_PROXIMITY = 0x19,
    SENSOR_CHANNEL_HUMIDITY = 0x78,
    SENSOR_CHANNEL_TEMPERATURE = 0xCC,
    SENSOR_CHANNEL_RNG = 0x666,
    // Add more channels as needed
} sensor_channel_t;
```

### Sensor Status

An enumeration representing the possible status codes for sensor operations.

```c
typedef enum {
    SENSOR_API_STATUS_UNKNOWN,        ///< Status is unknown
    SENSOR_API_STATUS_INITIALIZED,    ///< API has been initialized
    SENSOR_API_STATUS_CHANNEL_OPENED, ///< Channel has been opened
    SENSOR_API_STATUS_OK,             ///< Operation completed successfully
    SENSOR_API_STATUS_UNINITIALIZED,  ///< API is not initialized
    SENSOR_API_STATUS_ERROR           ///< An error occurred
} ocre_sensors_status_t;
```

### Execution Environment

All sensor functions have an internal `wasm_exec_env_t` parameter that is used by the Ocre runtime. This parameter is NOT needed when calling these functions from within a container. Container applications should omit this parameter.

---

## Methods

### Sensor Initialization

Initializes the sensors subsystem. This function must be called before using any other sensor functions to set up the necessary internal structures and prepare the system for sensor operations.

```c
int ocre_sensors_init(int unused);
```

**Parameters**:

| Name | Type | Description |
| ------ | ------ | ----------- |
| `unused` | *int* | Reserved parameter (not used) |

**Returns**:

| Value | Description |
| ------ | ----------- |
| `0` | on success |
| negative value | on error |


### Discover Sensors

Discovers all available sensors. This function enumerates all available sensors and populates the sensor API context.

```c
ocre_sensors_status_t ocre_sensors_discover_sensors(ocre_sensor_t *sensors, int *sensors_count);
```

**Parameters**:

| Name | Type | Description |
| ------ | ------ | ----------- |
| `sensors` | *ocre_sensor_t** | Pointer to the sensor list to store the discovered info about available sensors |
| `sensors_count` | *int** | Pointer to variable to store the number of discovered sensors |

**Returns**:

| Value | Description |
| ------ | ----------- |
| `SENSOR_API_STATUS_OK` | on success |
| other status code | on error |

### Open Sensor

Opens a sensor, allowing the system to begin interacting with it to obtain data. Typically, this involves configuring hardware or setting up the necessary communication protocols to start receiving data from the sensor.

```c
ocre_sensors_status_t ocre_sensors_open(ocre_sensor_handle_t *sensor_handle);
```

**Parameters**:

| Name | Type | Description |
| ------ | ------ | ----------- |
| `sensor_handle` | *ocre_sensor_handle_t** | Pointer to the sensor handle to open |

**Returns**:

| Value | Description |
| ------ | ----------- |
| `SENSOR_API_STATUS_OK` | on success |
| `SENSOR_API_STATUS_ERROR` | on error |


### Read Sensor Sample

Reads a sample from the specified sensor. The function retrieves the most recent data sample from the sensor identified by the provided sensor handle. The returned sample contains the raw sensor data, which can later be processed or interpreted based on the sensor type (**e.g.**, temperature, acceleration).

```c
ocre_sensors_sample_t sensor_read_sample(ocre_sensor_handle_t *sensor_handle);
```

**Parameters**:

| Name | Type | Description |
| ------ | ------ | ----------- |
| `sensor_handle` | *ocre_sensor_handle_t** | Pointer to the sensor handle to read from |

**Returns**:

| Value | Description |
| ------ | ----------- |
| `ocre_sensors_sample_t` | A structure containing the sensor data sample |


### Get Sensor Channel 

Retrieves data for a specific channel from a sensor sample. A sample may contain multiple channels of data, and this function helps in isolating the desired channel's data. For example, you can extract temperature, pressure, or acceleration values from a multi-function sensor.

```c
ocre_sensor_value_t sensor_get_channel(ocre_sensors_sample_t sample, sensor_channel_t channel);
```

**Parameters**:

| Name | Type | Description |
| ------ | ------ | ----------- |
| `sample` | *ocre_sensors_sample_t* | The sensor sample containing raw data |
| `channel` | *sensor_channel_t* | The specific channel to retrieve data for (e.g., `SENSOR_CHANNEL_TEMPERATURE`) |

**Returns**:

| Value | Description |
| ------ | ----------- |
| `ocre_sensor_value_t` | The value from the requested channel |

{: .note }
If the channel is invalid or unavailable, the function may return an error code or placeholder value.

### Set Sensor Trigger

Configures a trigger for a specific sensor channel. This function configures a specific trigger type (such as data ready or threshold) for a specific sensor channel. When the trigger condition is met, the provided callback function will be called.

```c
ocre_sensors_status_t ocre_sensors_set_trigger(ocre_sensor_handle_t sensor_handle, sensor_channel_t channel,
                                              enum sensor_trigger_type trigger_type, ocre_sensor_trigger_cb callback,
                                              int *subscription_id);
```

**Parameters**:

| Name | Type | Description |
| ------ | ------ | ----------- |
| `sensor_handle` | *ocre_sensor_handle_t* | Handle of the sensor |
| `channel` | *sensor_channel_t* | Channel to set the trigger on |
| `trigger_type` | *enum sensor_trigger_type* | Type of trigger (e.g., data ready, threshold) |
| `callback` | *ocre_sensor_trigger_cb* | Function to be called when trigger occurs |
| `subscription_id` | *int** | Pointer to store the subscription ID |

**Returns**:

| Value | Description |
| ------ | ----------- |
| `SENSOR_API_STATUS_OK` | on success |
| other status code | on error |

### Clear Sensor Trigger

Removes a previously set trigger from a sensor channel. This function stops the monitoring of the specified trigger and removes the associated callback.

```c
ocre_sensors_status_t ocre_sensors_clear_trigger(ocre_sensor_handle_t sensor_handle, sensor_channel_t channel,
                                                int subscription_id);
```

**Parameters**:

| Name | Type | Description |
| ------ | ------ | ----------- |
| `sensor_handle` | *ocre_sensor_handle_t* | Handle of the sensor |
| `channel` | *sensor_channel_t* | The specific channel (e.g., `SENSOR_CHANNEL_TEMPERATURE`) from which the trigger should be removed |
| `subscription_id` | *int* | ID of the subscription to remove |

**Returns**:

| Value | Description |
| ------ | ----------- |
| `SENSOR_API_STATUS_OK` | on success |
| other status code | on error |


### Clean Up Sensor Environment

Performs cleanup of the sensor subsystem. This function releases resources, closes any active sensor channels, and stops any background operations related to sensors. It should be called when the sensor subsystem is no longer needed, ensuring that system resources are properly freed and that the sensor environment is in a consistent state.

```c
ocre_sensors_status_t ocre_sensors_cleanup();
```

**Returns**:

| Value | Description |
| ------ | ----------- |
| `SENSOR_API_STATUS_OK` | on success |
| other status code | on error |

---

## Error Handling

Most functions return status codes from the `ocre_sensors_status_t` enumeration. Check the return value to determine if an operation succeeded or failed.

| Value | Description |
| ------ | ----------- |
| `SENSOR_API_STATUS_OK` | Operation completed successfully |
| `SENSOR_API_STATUS_ERROR` | General failure |
| `SENSOR_API_STATUS_UNINITIALIZED` | API not initialized |
| `SENSOR_API_STATUS_UNKNOWN` | Unknown error |

---

## Examples

### Basic Sensor Usage

```c
#include <stdio.h>
#include "ocre_sensors.h"

int main() {
    // Initialize the sensors subsystem
    if (ocre_sensors_init(0) != 0) {
        printf("Failed to initialize sensors\n");
        return -1;
    }
    
    // Discover available sensors
    ocre_sensor_t sensors[MAX_SENSORS];
    int sensor_count = 0;
    
    if (ocre_sensors_discover_sensors(sensors, &sensor_count) != SENSOR_API_STATUS_OK) {
        printf("Failed to enumerate sensors\n");
        return -1;
    }
    
    printf("Discovered %d sensors\n", sensor_count);
    
    // Open and read from first temperature sensor found
    for (int i = 0; i < sensor_count; i++) {
        // Look for a temperature sensor
        for (int j = 0; j < sensors[i].num_channels; j++) {
            if (sensors[i].channels[j] == SENSOR_CHANNEL_TEMPERATURE) {
                // Open the sensor
                if (ocre_sensors_open(&sensors[i].handle) != SENSOR_API_STATUS_OK) {
                    printf("Failed to open temperature sensor\n");
                    continue;
                }
                
                // Read a sample
                ocre_sensors_sample_t sample = sensor_read_sample(&sensors[i].handle);
                
                // Get temperature value
                ocre_sensor_value_t temp = sensor_get_channel(sample, SENSOR_CHANNEL_TEMPERATURE);
                
                // Print temperature (integer + fractional parts)
                printf("Temperature: %d.%d C\n", temp.integer, temp.floating);
                break;
            }
        }
    }
    
    // Clean up
    ocre_sensors_cleanup();
    return 0;
}
```

### Using Sensor Triggers

```c
#include <stdio.h>
#include "ocre_sensors.h"

// Global flag to indicate we're still running
volatile bool running = true;

// Trigger callback function
void temperature_callback(ocre_sensor_handle_t sensor_handle, sensor_channel_t channel,
                          ocre_sensor_value_t *data, void *ptr) {
    printf("Temperature event: %d.%d C\n", data->integer, data->floating);
    
    // If temperature exceeds threshold, stop monitoring
    if (data->integer > 30) {
        running = false;
    }
}

int main() {
    // Initialize sensors
    ocre_sensors_init(0);
    
    ocre_sensor_t sensors[MAX_SENSORS];
    int sensor_count = 0;
    
    if (ocre_sensors_discover_sensors(sensors, &sensor_count) != SENSOR_API_STATUS_OK) {
        printf("Failed to discover sensors\n");
        return -1;
    }
    
    // Find first temperature sensor
    for (int i = 0; i < sensor_count; i++) {
        for (int j = 0; j < sensors[i].num_channels; j++) {
            if (sensors[i].channels[j] == SENSOR_CHANNEL_TEMPERATURE) {
                // Open sensor
                if (ocre_sensors_open(&sensors[i].handle) == SENSOR_API_STATUS_OK) {
                    // Set up a trigger for temperature threshold
                    int subscription_id;
                    if (ocre_sensors_set_trigger(sensors[i].handle, SENSOR_CHANNEL_TEMPERATURE,
                                              SENSOR_TRIGGER_THRESHOLD, temperature_callback,
                                              &subscription_id) == SENSOR_API_STATUS_OK) {
                        
                        printf("Temperature monitoring active...\n");
                        
                        // Main application loop
                        while (running) {
                            // Application logic here
                            // The callback will be triggered automatically
                            k_sleep(K_MSEC(100));
                        }
                        
                        // Clean up trigger
                        ocre_sensors_clear_trigger(sensors[i].handle, SENSOR_CHANNEL_TEMPERATURE, 
                                                subscription_id);
                    }
                }
                break;
            }
        }
    }
    
    ocre_sensors_cleanup();
    return 0;
}
```

---

## Reference

| Function | Description | Parameters | Return Value | Status Codes |
|----------|-------------|------------|--------------|--------------|
| [`ocre_sensors_init`](#sensor-initialization) | Initializes sensors subsystem | `unused`: Reserved parameter | 0 on success, negative on error | N/A |
| [`ocre_sensors_discover_sensors`](#discover-sensors) | Discovers all available sensors | `sensors`: Pointer to the sensor list to store the discovered info about available sensors<br/>`sensors_count`: Counter to save the number of all found sensors | Status code | `SENSOR_API_STATUS_OK`,<br/>`SENSOR_API_STATUS_ERROR` |
| [`ocre_sensors_open`](#open-sensor) | Opens sensor | `sensor_handle`: Sensor to open | Status code | `SENSOR_API_STATUS_OK`,<br/>`SENSOR_API_STATUS_ERROR` |
| [`sensor_read_sample`](#read-sensor-sample) | Reads sensor data | `sensor_handle`: Sensor to read from | `ocre_sensors_sample_t` structure | N/A |
| [`sensor_get_channel`](#get-sensor-channel) | Gets channel data | `sample`: Sensor sample<br/>`channel`: Channel to retrieve | `ocre_sensor_value_t` structure | N/A |
| [`ocre_sensors_set_trigger`](#set-sensor-trigger) | Sets sensor trigger | `sensor_handle`: Target sensor<br/>`channel`: Target channel<br/>`trigger_type`: Trigger type<br/>`callback`: Callback function<br/>`subscription_id`: ID output | Status code | `SENSOR_API_STATUS_OK`,<br/>`SENSOR_API_STATUS_ERROR` |
| [`ocre_sensors_clear_trigger`](#clear-sensor-trigger) | Removes sensor trigger | `sensor_handle`: Target sensor<br/>`channel`: Target channel<br/>`subscription_id`: Subscription to remove | Status code | `SENSOR_API_STATUS_OK`,<br/>`SENSOR_API_STATUS_ERROR` |
| [`ocre_sensors_cleanup`](#clean-up-sensor-environment) | Cleans up resources | None | Status code | `SENSOR_API_STATUS_OK`,<br/>`SENSOR_API_STATUS_ERROR` |