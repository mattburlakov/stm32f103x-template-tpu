cmake_minimum_required(VERSION 4.0.0)

include(${CMAKE_SOURCE_DIR}/cmake/arm_none_eabi.cmake)

project(firmware C)

enable_language(C ASM)
set(CMAKE_C_STANDARD 17)
set(CMAKE_C_STANDARD_REQUIRED ON)

set(EXECUTABLE ${PROJECT_NAME}.elf)

add_subdirectory(${CMAKE_SOURCE_DIR}/vendor/CMSIS)

add_executable(${EXECUTABLE}
    ${CMAKE_SOURCE_DIR}/source.c
    ${CMAKE_SOURCE_DIR}/startup_stm32f103rctx.s
)
target_link_libraries(${EXECUTABLE} cmsis)

target_link_options(${EXECUTABLE} PRIVATE
    -T${CMAKE_SOURCE_DIR}/STM32F103RCTX_FLASH.ld
    -mcpu=cortex-m3

    -mthumb
    -mfloat-abi=soft
    
    -specs=nano.specs

    -Wl,-Map=${PROJECT_NAME}.map,--cref
    -Wl,--gc-sections
    -Wl,--print-memory-usage
)

target_compile_options(${EXECUTABLE} PRIVATE

    -mcpu=cortex-m3
    -mthumb
    -mfloat-abi=soft

    -g3 

    -DSTM32F103xx
    -DSTM32F10X_MD
    -DSTM32F103RBT

    -fdata-sections
    -ffunction-sections

    -Wall

    -O
)

add_custom_command(TARGET ${EXECUTABLE}
    POST_BUILD
    COMMAND arm-none-eabi-size ${EXECUTABLE}
)

add_custom_command(TARGET ${EXECUTABLE}
    POST_BUILD
    COMMAND arm-none-eabi-objcopy -Oihex ${EXECUTABLE} ${PROJECT_NAME}.hex
    COMMAND arm-none-eabi-objcopy -Obinary ${EXECUTABLE} ${PROJECT_NAME}.bin
    COMMAND arm-none-eabi-objdump -D ${EXECUTABLE} > ${PROJECT_NAME}.txt
    COMMAND 
)
