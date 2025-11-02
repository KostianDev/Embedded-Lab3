# Embedded Lab 3 - TIM4 PWM LEDs

STM32F407 project that drives four on-board LEDs from TIM4 PWM channels. All LEDs blink at the same carrier frequency while keeping independent duty cycles chosen for variant **#3**. The firmware is generated with STM32CubeMX and built with the supplied Makefile.

## Hardware
- MCU: STM32F407VGT6
- LEDs (active-high, alternate function TIM4):
  - `PD12` - LED_GREEN --> TIM4_CH1
  - `PD13` - LED_ORANGE (LED_YELLOW in my version) --> TIM4_CH2
  - `PD14` - LED_RED --> TIM4_CH3
  - `PD15` - LED_BLUE --> TIM4_CH4
- Debug/programming interface: SWD (ST-LINK)

## PWM timing targets (variant 3)
All channels share the same timer period to yield a **4.41 kHz** PWM frequency. Duty cycles per LED:

| LED | Duty cycle |
| --- | ---------- |
| LED_RED (TIM4_CH3) | 57 % |
| LED_GREEN (TIM4_CH1) | 89 % |
| LED_ORANGE / LED_YELLOW (TIM4_CH2) | 24 % |
| LED_BLUE (TIM4_CH4) | 50 % |

Timer prescaler, auto-reload and compare values are preconfigured in `Core/Src/tim.c` so the PWM outputs match the table without any run-time math. The main application simply starts PWM on all four channels after initialization.

## Build
```bash
make
```
Artifacts are placed under `build/` (e.g., `build/Lab3.bin`).

## Flash
Use any preferred SWD programmer. Examples:

- STM32CubeProgrammer CLI:
  ```bash
  STM32_Programmer_CLI -c port=SWD -w build/Lab3.bin 0x08000000 -v -rst
  ```
- ST-LINK command-line:
  ```bash
  st-flash --reset write build/Lab3.bin 0x08000000
  ```

## Run
On reset the PWM outputs become active immediately. LED brightness reflects the duty cycles listed above. There is no button control in this lab; the timer free-runs in hardware.
