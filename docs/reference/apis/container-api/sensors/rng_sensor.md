---
title: RNG Sensor
layout: default
parent: Sensors 
---

# RNG Sensor
{: .no_toc }

The RNG sensor is pre-integrated with the Ocre sensor subsystem and built into the standard Ocre runtime. Users can access it through the [general Sensors](general_sensors.md) API without needing any additional header files, initialization steps, or configuration.

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
#include "rng_sensor.h"
```

{: .highlight Note}
The RNG sensor header file is for internal use only in the Ocre runtime implementation. Application developers should NOT include this header file in their code. Instead, they should only include the general sensors API header (`ocre_sensors.h`) and access the RNG sensor through the [general Sensors](general_sensors.md) API.

---

## Methods

For end users, there are no additional methods specific to the RNG sensor beyond those provided by the general Sensors API. The RNG sensor is accessed entirely through the standard sensor interface.

The internal RNG sensor implementation includes:

### RNG Sensor Initialization

```c
int rng_sensor_init(const struct device *dev);
```

This function is for internal use only and is automatically called during system initialization. Users do not need to call this function.

---

## Usage with Sensors API

The RNG sensor provides a random number generation channel that can be accessed through the standard Sensors API. 

**To use the RNG sensor:**

1. Include only the general sensors header: `#include "ocre_sensors.h"`
2. Initialize the sensor subsystem with `ocre_sensors_init()`
3. Discover available sensors with `ocre_sensors_discover()`
4. Find the RNG sensor by looking for a sensor that supports the channel `SENSOR_CHAN_CUSTOM + 1` (value 2)
5. Get a handle and open the sensor using standard API calls
6. Read random values using `ocre_sensors_read()` with channel value `SENSOR_CHAN_CUSTOM + 1`

---

## Examples 

### Basic RNG Sensor Usage

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
    int sensor_count = ocre_sensors_discover();
    if (sensor_count <= 0) {
        printf("Failed to discover sensors\n");
        return -1;
    }
    
    printf("Discovered %d sensors\n", sensor_count);
    
    // Find RNG sensor
    int rng_sensor_id = -1;
    for (int i = 0; i < sensor_count; i++) {
        int channel_count = ocre_sensors_get_channel_count(i);
        
        // Check each channel of this sensor
        for (int j = 0; j < channel_count; j++) {
            int channel_type = ocre_sensors_get_channel_type(i, j);
            
            // Check if this is an RNG channel (using SENSOR_CHAN_CUSTOM + 1 value)
            if (channel_type == 2) { // SENSOR_CHAN_CUSTOM + 1 = 2
                rng_sensor_id = i;
                break;
            }
        }
        
        if (rng_sensor_id >= 0) break;
    }
    
    if (rng_sensor_id < 0) {
        printf("RNG sensor not found\n");
        return -1;
    }
    
    // Get handle for the RNG sensor
    ocre_sensor_handle_t rng_handle = ocre_sensors_get_handle(rng_sensor_id);
    if (rng_handle < 0) {
        printf("Failed to get handle for RNG sensor\n");
        return -1;
    }
    
    // Open the RNG sensor
    if (ocre_sensors_open(rng_handle) != 0) {
        printf("Failed to open RNG sensor\n");
        return -1;
    }
    
    // Read a random value, using SENSOR_CHAN_CUSTOM + 1 (value 2) as the channel
    int random_value = ocre_sensors_read(rng_sensor_id, 2); // Channel 2 is the RNG channel
    if (random_value < 0) {
        printf("Failed to read random value\n");
        return -1;
    }
    
    printf("Random value: %d\n", random_value);
    
    return 0;
}
```