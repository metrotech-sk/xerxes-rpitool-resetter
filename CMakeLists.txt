cmake_minimum_required(VERSION 3.22)

include(pico_sdk_import.cmake)
# We also need PICO EXTRAS
include(pico_extras_import.cmake)

set(PROJECT_NAME sensor-rp2040)


# this is the board we are using, we need to set this to use the correct board header and bootloader
# the board file is: src/Hardware/Board/xerxes_rp2040.h
set(PICO_BOARD "pink")
set(PICO_BOARD_HEADER_DIRS "${CMAKE_CURRENT_LIST_DIR}/src/Board")


project(${PROJECT_NAME} C CXX ASM)
set(CMAKE_CXX_STANDARD 23)

pico_sdk_init()
	

include_directories(
	"src"
)

add_executable(
	${PROJECT_NAME}

	src/main.cpp
)

target_link_libraries(
	${PROJECT_NAME} 
	pico_stdlib 
	pico_multicore
	hardware_rtc
	hardware_gpio
	hardware_adc
	hardware_uart
	hardware_pwm
	hardware_sleep
	hardware_flash
	hardware_sync
	hardware_spi
)

# enable usb output, disable uart output
pico_enable_stdio_usb(${PROJECT_NAME} 1)
pico_enable_stdio_uart(${PROJECT_NAME} 0)
# pico_enable_stdio_uart(${PROJECT_NAME} 1)

pico_add_extra_outputs(${PROJECT_NAME})

# copy .UF2 binary to the RP2040
install(
	FILES
	${CMAKE_CURRENT_BINARY_DIR}/${PROJECT_NAME}.uf2
	DESTINATION /media/$ENV{USER}/RPI-RP2/${PROJECT_NAME}.uf2
)