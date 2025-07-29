# STM Cassaforte BT 🔐

A simple secure box project using STM32 microcontroller, UART communication and PWM control for servo actuation.

## Overview

This project implements a basic safe ("cassaforte") mechanism:
- The user sends a 4-digit combination via UART.
- If the combination is correct, a servo motor toggles between 0° and 90°.
- A green LED blinks on success, while a red LED blinks on failure.

## Hardware Requirements

- STM32 board (e.g., STM32F1 series)
- SG90 or compatible servo motor
- 2 LEDs (for PB0 and PB1)
- UART to USB interface (e.g., USB-TTL adapter)
- Power supply (e.g., USB or external 5V)
- STM32CubeIDE

## PWM Configuration (TIM2)

- **Prescaler**: 72-1 → 1 MHz timer clock
- **Period**: 20000-1 → 20 ms PWM period
- **Pulse values**:
  - `1000` → 0° position (1 ms pulse width)
  - `2000` → 90° position (2 ms pulse width)

These values correspond to the standard servo control PWM signals.

## PWM Code Example

```c
__HAL_TIM_SET_COMPARE(&htim2, TIM_CHANNEL_1, 2000); // 90°
__HAL_TIM_SET_COMPARE(&htim2, TIM_CHANNEL_1, 1000); // 0°
```

## How It Works

The microcontroller waits for 4 characters over UART.  
If the input is `"1234"`, it toggles the servo between 0° and 90° and blinks the green LED (PB1).  
If the code is incorrect, the red LED (PB0) blinks instead.  
UART reception is handled using HAL callbacks and interrupt-based processing.

## Getting Started

1. Clone this repository  
2. Open the project in STM32CubeIDE  
3. Flash the firmware to your STM32 board  
4. Open a serial monitor (9600 baud, 8N1)  
5. Type `1234` to unlock/lock the servo  

## Author

Developed by [itsdom1](https://github.com/itsdom1)  
License: [MIT](https://choosealicense.com/licenses/mit/)
