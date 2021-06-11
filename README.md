# stm32cubemx-example

Example project utilising auto-generated code from STM32CubeMX.

## Build Prerequisites

- CMake version 3.16 or later
- arm-none-eabi-gcc

## Directory Structure

- **cubemx** : Contains the STM32CubeMX project for automatic code generation.
- **doc**    : Documentation.
- **src**    : Source code.
  - **Core**    : Auto generated source code.
  - **Drivers** : Third-party drivers (CMSIS, ST HAL).
- **test**   : Tests.
