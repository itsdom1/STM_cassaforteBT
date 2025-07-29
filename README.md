# 🔐 STM32 Safe Lock with Servo and UART

This project simulates a simple **electronic safe** using an STM32 microcontroller. It listens for a 4-digit code via UART. If the correct code `1234` is received, a **servo motor** rotates to simulate unlocking, and a green LED blinks. If the code is wrong, a red LED blinks and the servo remains locked.

## Features

- UART4 serial communication via interrupt (`HAL_UART_Receive_IT`)
- PWM control of a servo motor using TIM2
- Code validation and feedback through LEDs
- Uses STM32 HAL libraries

## Hardware Required

- STM32 development board (e.g. STM32F103C8T6)
- SG90 or compatible servo motor
- 2 LEDs (connected to PB0 and PB1)
- USB-UART adapter (if needed)
- 5V power supply (for servo)

## PWM Configuration

The servo is driven using PWM with these settings:

- **Prescaler**: 71 → 1 MHz timer clock
- **Period**: 19999 → 20 ms PWM cycle (50 Hz)
- **Pulse width**:
  - `1000` → 1.0 ms → 0°
  - `2000` → 2.0 ms → 90°

```c
__HAL_TIM_SET_COMPARE(&htim2, TIM_CHANNEL_1, 1000); // 0°
__HAL_TIM_SET_COMPARE(&htim2, TIM_CHANNEL_1, 2000); // 90°
How It Works
The microcontroller waits for 4 characters over UART.

If the input is "1234", it toggles the servo angle between 0° and 90° and blinks the green LED (PB1).

If the code is incorrect, the red LED (PB0) blinks twice.

UART reception is handled using HAL callbacks and interrupt-based reception.

Getting Started
Clone the repository

Open the project in STM32CubeIDE

Flash it to your STM32 board

Open a serial monitor (e.g. PuTTY, TeraTerm) at 9600 baud

Type 1234 and observe servo + LED behavior

Author
Developed by itsdom1
License: MIT
