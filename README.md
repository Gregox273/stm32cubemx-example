# stm32cubemx-example

Example project utilising auto-generated code from STM32CubeMX.

Includes VSCode configuration for developing within a containerised Linux environment, specifically:

https://github.com/Gregox273/stm32-dev-environment/

## Build Prerequisites (if not using Docker)

- CMake version 3.16 or later
- arm-none-eabi-gcc toolchain

## Building

If using VSCode, these commands are configured as tasks in tasks.json .

0. (If not done already) Generate CMake project output:
  
    `cmake -G \"Unix Makefiles\" -DCMAKE_TOOLCHAIN_FILE=../arm-none-eabi-gcc.cmake -DCMAKE_BUILD_TYPE=Debug ..`

1. Run build command:

    `cmake --build .`

## Directory Structure

- **cubemx** : Contains the STM32CubeMX project for automatic code generation.
- **doc**    : Documentation.
- **src**    : Source code.
  - **Core**    : Auto generated source code.
  - **Drivers** : Third-party drivers (CMSIS, ST HAL).
- **test**   : Tests.
