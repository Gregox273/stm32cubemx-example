# Based on example by Pierre Gradot:
# https://dev.to/younup/cmake-on-stm32-the-beginning-3766
cmake_minimum_required(VERSION 3.16)

project(stm32cubemx-example)

enable_language(C ASM)

set(CMAKE_C_STANDARD 99)
set(CMAKE_C_STANDARD_REQUIRED ON)

# Sources
set(STM32CUBEMX_GENERATED_FILES
    src/Core/Src/main.c

    src/Core/Inc/main.h

    src/Core/Src/gpio.c
    src/Core/Src/usart.c

    src/Core/Inc/gpio.h
    src/Core/Inc/usart.h

    src/Core/Src/stm32f4xx_it.c
    src/Core/Src/stm32f4xx_hal_msp.c
    src/Core/Src/system_stm32f4xx.c

    src/Core/Inc/stm32f4xx_hal_conf.h
    src/Core/Inc/stm32f4xx_it.h

    src/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_tim_ex.c
    src/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_uart.c
    src/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_rcc.c
    src/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_rcc_ex.c
    src/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_flash.c
    src/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_flash_ex.c
    src/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_flash_ramfunc.c
    src/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_tim.c
    src/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_gpio.c
    src/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_dma_ex.c
    src/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_dma.c
    src/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_pwr.c
    src/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_pwr_ex.c
    src/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_cortex.c
    src/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal.c
    src/Drivers/STM32F4xx_HAL_Driver/Src/stm32f4xx_hal_exti.c

    startup_stm32f401xe.s
)

# Sources
set(C_SOURCES

)

set(EXECUTABLE ${PROJECT_NAME}.elf)

add_executable(${EXECUTABLE} ${STM32CUBEMX_GENERATED_FILES} ${C_SOURCES})

# C defines
target_compile_definitions(${EXECUTABLE} PRIVATE
    -DUSE_HAL_DRIVER
    -DSTM32F401xE
)

# C includes
target_include_directories(${EXECUTABLE} PRIVATE
    src/Core/Inc
    src/Drivers/STM32F4xx_HAL_Driver/Inc
    src/Drivers/STM32F4xx_HAL_Driver/Inc/Legacy
    src/Drivers/CMSIS/Device/ST/STM32F4xx/Include
    src/Drivers/CMSIS/Include
)

# CFLAGS
target_compile_options(${EXECUTABLE} PRIVATE
    -mcpu=cortex-m4
    -mfpu=fpv4-sp-d16
    -mfloat-abi=hard
    -mthumb
    -Wall
    -fdata-sections
    -ffunction-sections

    $<$<CONFIG:Debug>:-Og -g -gdwarf-2>
)

target_link_options(${EXECUTABLE} PRIVATE
    -specs=nano.specs    
    -T${CMAKE_SOURCE_DIR}/STM32F401RETx_FLASH.ld
    -mcpu=cortex-m4
    -mfpu=fpv4-sp-d16
    -mfloat-abi=hard
    -mthumb
    -lc
    -lm
    -lnosys
    -Wl,-Map=${PROJECT_NAME}.map,--cref
    -Wl,--gc-sections
    -Wl,--print-memory-usage
)

# Print executable size
add_custom_command(TARGET ${EXECUTABLE}
    POST_BUILD
    COMMAND arm-none-eabi-size ${EXECUTABLE})

# Create hex and binary files
add_custom_command(TARGET ${EXECUTABLE}
    POST_BUILD
    COMMAND arm-none-eabi-objcopy -O ihex ${EXECUTABLE} ${PROJECT_NAME}.hex
    COMMAND arm-none-eabi-objcopy -O binary ${EXECUTABLE} ${PROJECT_NAME}.bin
)