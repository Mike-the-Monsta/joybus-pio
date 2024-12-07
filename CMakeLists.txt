cmake_minimum_required(VERSION 3.13)

# Pull in SDK (must be before project)
include(pico_sdk_import.cmake)

project(joybus_pio VERSION 1.0.0 LANGUAGES C CXX ASM)

# Initialize the Raspberry Pi Pico SDK
pico_sdk_init()

# Add library target
add_library(joybus_pio
    src/joybus.c
    src/GamecubeConsole.cpp
    src/GamecubeController.cpp
    src/N64Console.cpp
    src/N64Controller.cpp
)

# Generate PIO header
pico_generate_pio_header(joybus_pio ${CMAKE_CURRENT_LIST_DIR}/src/joybus.pio)

target_include_directories(joybus_pio PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/include
)

# Set C++ standard
target_compile_features(joybus_pio PUBLIC cxx_std_11)

# Link against pico libraries
target_link_libraries(joybus_pio PUBLIC
    pico_stdlib
    hardware_pio
)

# Make examples optional and disabled by default
option(JOYBUS_BUILD_EXAMPLES "Build joybus-pio examples" OFF)
if(JOYBUS_BUILD_EXAMPLES)
    add_subdirectory(examples)
endif()

# Enable USB output, disable UART output
pico_enable_stdio_usb(joybus_pio 1)
pico_enable_stdio_uart(joybus_pio 0)
