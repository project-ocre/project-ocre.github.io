---
title: GPIO
layout: default
parent: Container APIs
has_toc: false 
---

# GPIO
{: .no_toc }

The GPIO API provides access to General Purpose Input/Output pins on Ocre containers. It enables applications to configure pins, read input states, set output states, and register callbacks for pin state changes.

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
#include "ocre_gpio.h"
```

---

## Types

### GPIO Pin State

An enumeration defining the possible states of a GPIO pin.

```c
typedef enum {
    OCRE_GPIO_PIN_RESET = 0,  ///< Pin is low (0)
    OCRE_GPIO_PIN_SET = 1     ///< Pin is high (1)
} ocre_gpio_pin_state_t;
```

### GPIO Direction

An enumeration defining the direction of a GPIO pin.

```c
typedef enum {
    OCRE_GPIO_DIR_INPUT = 0,   ///< Pin configured as input
    OCRE_GPIO_DIR_OUTPUT = 1   ///< Pin configured as output
} ocre_gpio_direction_t;
```

### GPIO Configuration

A structure for configuring a GPIO pin.

```c
typedef struct {
    int pin;                         /**< GPIO pin number (logical) */
    ocre_gpio_direction_t direction; /**< Pin direction */
} ocre_gpio_config_t;
```

### GPIO Callback Function

A function that will be called when a GPIO pin's state changes.

```c
typedef void (*ocre_gpio_callback_t)(int pin, ocre_gpio_pin_state_t state);
```

### Execution Environment

All GPIO functions have an internal `wasm_exec_env_t` parameter that is used by the Ocre runtime. This parameter is NOT needed when calling these functions from within a container. Container applications should omit this parameter.

---

## Methods

### GPIO Initialization

Initializes the GPIO subsystem. This function must be called before using any other GPIO functions.

```c
int ocre_gpio_init(void);
```

**Returns**:

| Value | Description |
| ------ | ----------- |
| `0` | on success |
| negative value | on error |

### Configure GPIO Pin

Configures a GPIO pin for either input or output.

```c
int ocre_gpio_configure(const ocre_gpio_config_t *config);
```

**Parameters**:

| Name | Type | Description |
| ------ | ------ | ----------- |
| `config` | *const ocre_gpio_config_t* * | Pointer to a GPIO configuration structure |

**Returns**:

| Value | Description |
| ------ | ----------- |
| `0` | on success |
| negative value | on error |

### Set GPIO Pin

Sets the state of a GPIO output pin.

```c
int ocre_gpio_pin_set(int pin, ocre_gpio_pin_state_t state);
```

**Parameters**:

| Name | Type | Description |
| ------ | ------ | ----------- |
| `pin` | *int* | GPIO pin number |
| `state` | *ocre_gpio_pin_state_t* | State to set the pin to (`OCRE_GPIO_PIN_SET` or `OCRE_GPIO_PIN_RESET`) |

**Returns**:

| Value | Description |
| ------ | ----------- |
| `0` | on success |
| negative value | on error |

### Get GPIO Pin State

Reads the current state of a GPIO pin.

```c
ocre_gpio_pin_state_t ocre_gpio_pin_get(int pin);
```

**Parameters**:

| Name | Type | Description |
| ------ | ------ | ----------- |
| `pin` | *int* | GPIO pin number |

**Returns**:

| Value | Description |
| ------ | ----------- |
| `OCRE_GPIO_PIN_SET` | if the pin is high |
| `OCRE_GPIO_PIN_RESET` | if the pin is low |
|  negative value | on error |

### Toggle GPIO Pin

Toggles the state of a GPIO output pin.

```c
int ocre_gpio_pin_toggle(int pin);
```

**Parameters**:

| Name | Type | Description |
| ------ | ------ | ----------- |
| `pin` | *int* | GPIO pin number |

**Returns**:

| Value | Description |
| ------ | ----------- |
| `0` | on success |
| negative value | on error |

### Register GPIO Callback

Registers a callback function to be called when a GPIO input pin changes state.

```c
int ocre_gpio_register_callback(int pin, ocre_gpio_callback_t callback);
```

**Parameters**:

| Name | Type | Description |
| ------ | ------ | ----------- |
| `pin` | *int* | GPIO pin number |
| `callback` | *ocre_gpio_callback_t* | Function to call when the pin state changes |

**Returns**:

| Value | Description |
| ------ | ----------- |
| `0` | on success |
| negative value | on error |

### Unregister GPIO Callback

Removes a previously registered callback for a GPIO pin.

```c
int ocre_gpio_unregister_callback(int pin);
```

**Parameters**:

| Name | Type | Description |
| ------ | ------ | ----------- |
| `pin` | *int* | GPIO pin number |

**Returns**:

| Value | Description |
| ------ | ----------- |
| `0` | on success |
| negative value | on error |

### Get Pin ID

Calculates a pin ID from port and pin numbers.

```c
int get_pin_id(int port, int pin);
```

**Parameters**:

| Name | Type | Description |
| ------ | ------ | ----------- |
| `port` | *int* | GPIO port number |
| `pin` | *int* | Pin number within the port |

**Returns**:

| Value | Description |
| ------ | ----------- |
| positive value (pin ID) | on success |
| `-1` | on error |

---

## Error Handling

Most GPIO functions return status codes:

| Value | Description |
| ------ | ----------- |
| `0` | Operation completed successfully |
| `ENODEV` | GPIO system not initialized |
| `EINVAL` | Invalid parameter |
| `ENOMEM` | Memory allocation failed |
| other negative values | Other errors from underlying system |

---

## Examples

### Basic GPIO Output

```c
#include <stdio.h>
#include "ocre_gpio.h"

#define LED_PIN 5   // Example pin number (adjust for your hardware)

int main() {
    // Initialize GPIO subsystem
    if (ocre_gpio_init() != 0) {
        printf("Failed to initialize GPIO\n");
        return -1;
    }
    
    // Configure the pin as output
    ocre_gpio_config_t config;
    config.pin = LED_PIN;
    config.direction = OCRE_GPIO_DIR_OUTPUT;
    
    if (ocre_gpio_configure(&config) != 0) {
        printf("Failed to configure GPIO pin\n");
        return -1;
    }
    
    // Blink LED 5 times
    for (int i = 0; i < 5; i++) {
        // Turn LED on
        ocre_gpio_pin_set(LED_PIN, OCRE_GPIO_PIN_SET);
        printf("LED on\n");
        
        // Delay (implementation-specific)
        for (volatile int j = 0; j < 1000000; j++);
        
        // Turn LED off
        ocre_gpio_pin_set(LED_PIN, OCRE_GPIO_PIN_RESET);
        printf("LED off\n");
        
        // Delay (implementation-specific)
        for (volatile int j = 0; j < 1000000; j++);
    }
    
    return 0;
}
```

### GPIO Input with Callback

```c
#include <stdio.h>
#include <stdbool.h>
#include "ocre_gpio.h"

#define BUTTON_PIN 7   // Example pin number (adjust for your hardware)

// Flag to signal application to exit
volatile bool running = true;

// Callback function for button press
void button_callback(int pin, ocre_gpio_pin_state_t state) {
    printf("Button event on pin %d: state = %d\n", pin, state);
    
    // If button is pressed, exit the application
    if (state == OCRE_GPIO_PIN_SET) {
        printf("Button pressed, exiting...\n");
        running = false;
    }
}

int main() {
    // Initialize GPIO subsystem
    if (ocre_gpio_init() != 0) {
        printf("Failed to initialize GPIO\n");
        return -1;
    }
    
    // Configure the pin as input
    ocre_gpio_config_t config;
    config.pin = BUTTON_PIN;
    config.direction = OCRE_GPIO_DIR_INPUT;
    
    if (ocre_gpio_configure(&config) != 0) {
        printf("Failed to configure GPIO pin\n");
        return -1;
    }
    
    // Register callback for button presses
    if (ocre_gpio_register_callback(BUTTON_PIN, button_callback) != 0) {
        printf("Failed to register GPIO callback\n");
        return -1;
    }
    
    printf("Waiting for button press...\n");
    
    // Main application loop
    while (running) {
        // Application logic here
        
        // Simple delay
        for (volatile int j = 0; j < 100000; j++);
    }
    
    // Clean up
    ocre_gpio_unregister_callback(BUTTON_PIN);
    
    return 0;
}
```

### Using Port and Pin Mapping

```c
#include <stdio.h>
#include "ocre_gpio.h"

#define GPIO_PORT_A 0
#define GPIO_PIN_5  5

int main() {
    // Initialize GPIO subsystem
    if (ocre_gpio_init() != 0) {
        printf("Failed to initialize GPIO\n");
        return -1;
    }
    
    // Calculate pin ID from port and pin
    int pin_id = get_pin_id(GPIO_PORT_A, GPIO_PIN_5);
    if (pin_id < 0) {
        printf("Invalid port or pin\n");
        return -1;
    }
    
    // Configure the pin as output
    ocre_gpio_config_t config;
    config.pin = pin_id;
    config.direction = OCRE_GPIO_DIR_OUTPUT;
    
    if (ocre_gpio_configure(&config) != 0) {
        printf("Failed to configure GPIO pin\n");
        return -1;
    }
    
    // Toggle the pin 10 times
    for (int i = 0; i < 10; i++) {
        ocre_gpio_pin_toggle(pin_id);
        printf("Pin toggled\n");
        
        // Delay (implementation-specific)
        for (volatile int j = 0; j < 500000; j++);
    }
    
    return 0;
}
```

---

## Reference

| Function | Description | Parameters | Return Value | Error Codes |
|----------|-------------|------------|--------------|--------------|
| [`ocre_gpio_init`](#gpio-initialization) | Initializes GPIO subsystem | None | `0` on success, negative on error | `ENODEV` |
| [`ocre_gpio_configure`](#configure-gpio-pin) | Configures a GPIO pin | `config`: Pointer to GPIO configuration | `0` on success, negative on error | `ENODEV`, `EINVAL` |
| [`ocre_gpio_pin_set`](#set-gpio-pin) | Sets a GPIO pin state | `pin`: GPIO pin number<br/>`state`: Pin state to set | `0` on success, negative on error | `ENODEV`, `EINVAL` |
| [`ocre_gpio_pin_get`](#get-gpio-pin-state) | Gets a GPIO pin state | `pin`: GPIO pin number | `OCRE_GPIO_PIN_SET`/`OCRE_GPIO_PIN_RESET` or negative error code | `ENODEV`, `EINVAL` |
| [`ocre_gpio_pin_toggle`](#toggle-gpio-pin) | Toggles a GPIO pin state | `pin`: GPIO pin number | `0` on success, negative on error | `ENODEV`, `EINVAL` |
| [`ocre_gpio_register_callback`](#register-gpio-callback) | Registers callback for GPIO changes | `pin`: GPIO pin number<br/>`callback`: Callback function | `0` on success, negative on error | `ENODEV`, `EINVAL` |
| [`ocre_gpio_unregister_callback`](#unregister-gpio-callback) | Removes GPIO callback | `pin`: GPIO pin number | `0` on success, negative on error | `ENODEV`, `EINVAL` |
| [`get_pin_id`](#get-pin-id) | Calculates pin ID from port and pin | `port`: GPIO port number<br/>`pin`: Pin number within port | Pin ID or `-1` on invalid Pin | N/A |