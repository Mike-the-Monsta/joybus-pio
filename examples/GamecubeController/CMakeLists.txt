add_executable(
  GamecubeController
  main.cpp
)

# No need to generate PIO header here as it's done in the main library
# pico_generate_pio_header(GamecubeController ${CMAKE_CURRENT_LIST_DIR}/../../src/joybus.pio)

target_link_libraries(GamecubeController 
    joybus_pio
    pico_stdlib 
    hardware_pio
)

pico_enable_stdio_usb(GamecubeController 1)
pico_enable_stdio_uart(GamecubeController 0)

pico_add_extra_outputs(GamecubeController)
