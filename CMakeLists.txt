cmake_minimum_required(VERSION 3.13)

#
# Path to the source code to build
#
set(SRC_TO_BUILD_PATH ${CMAKE_CURRENT_LIST_DIR}/tinyusb/hw/mcu/raspberry_pi/Pico-PIO-USB/examples/host_hid_to_device_cdc)
#
# Uncomment the next line to build code in ./src
#set(SRC_TO_BUILD_PATH ${CMAKE_CURRENT_LIST_DIR}/src/host_hid_to_device_cdc)
#
# Then copy the whole host_hid_to_device_cdc folder to ./src
# cp -R ./tinyusb/hw/mcu/raspberry_pi/Pico-PIO-USB/examples/host_hid_to_device_cdc ./src/


#
# Include TinyUSB
#
if (NOT PICO_TINYUSB_PATH)
    if(DEFINED ENV{PICO_TINYUSB_PATH})
        set(PICO_TINYUSB_PATH $ENV{PICO_TINYUSB_PATH})
        message("Setting PICO_TINYUSB_PATH from environment ('${PICO_TINYUSB_PATH}')")
    else()
        set(PICO_TINYUSB_PATH ${CMAKE_CURRENT_LIST_DIR}/tinyusb)
        message("Setting PICO_TINYUSB_PATH from git submodule path ('${PICO_TINYUSB_PATH}')")
    endif()
else()
    message("PICO_TINYUSB_PATH is ('${PICO_TINYUSB_PATH}')")
endif()

if (NOT PICO_SDK_PATH)
    if(DEFINED ENV{PICO_SDK_PATH})
        set(PICO_SDK_PATH $ENV{PICO_SDK_PATH})
        message("Setting PICO_SDK_PATH from environment ('${PICO_SDK_PATH}')")
    else()
        set(PICO_SDK_PATH ${CMAKE_CURRENT_LIST_DIR}/pico-sdk)
        message("Setting PICO_SDK_PATH from git submodule path ('${PICO_SDK_PATH}')")
    endif()
else()
    message("PICO_SDK_PATH is ('${PICO_SDK_PATH}')")
endif()

set(FAMILY rp2040)

include(${PICO_SDK_PATH}/external/pico_sdk_import.cmake)
set(PROJECT minimal-pico-tinyusb-pio-project)

project(${PROJECT})

#
# Initialize Pico-SDK and TinyUSB
#
pico_sdk_init()

#
# TinyUSB stuff
#
set(PICO_PIO_USB_SRC "${PICO_TINYUSB_PATH}/hw/mcu/raspberry_pi/Pico-PIO-USB/src")
add_subdirectory(${SRC_TO_BUILD_PATH})
