cmake_minimum_required(VERSION 3.13)

#
# Optional
#
# To build code in ./src
#
# 1. Copy the whole host_hid_to_device_cdc folder to ./src
# 2. Comment out the line set(PICO_PIO_USB_SRC "${CMAKE_CURRENT_LIST_DIR}/../../src")
# 3. Uncomment the next line 
#set(SRC_TO_BUILD_PATH ${CMAKE_CURRENT_LIST_DIR}/src/host_hid_to_device_cdc)
#
# cp -R ./tinyusb/hw/mcu/raspberry_pi/Pico-PIO-USB/examples/host_hid_to_device_cdc ./src/
# sed --in-place -re 's/^set\(PICO_PIO_USB_SRC .*/# \0/' src/host_hid_to_device_cdc/CMakeLists.txt
# sed --in-place -re 's/^#(set\(SRC_TO_BUILD_PATH .*)/\1/' CMakeLists.txt
#


#
# Set path to Pico-sdk
#
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

#
# Set path to TinyUSB
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

#
# Set path to Pico-PIO-USB
#
if (NOT PICO_PIO_USB_PATH)
    if(DEFINED ENV{PICO_PIO_USB_PATH})
        set(PICO_PIO_USB_PATH $ENV{PICO_PIO_USB_PATH})
        message("Setting PICO_PIO_USB_PATH from environment ('${PICO_PIO_USB_PATH}')")
    else()
        set(PICO_PIO_USB_PATH ${PICO_TINYUSB_PATH}/hw/mcu/raspberry_pi/Pico-PIO-USB)
        message("Setting PICO_PIO_USB_PATH from git submodule path ('${PICO_PIO_USB_PATH}')")
    endif()
else()
    message("PICO_PIO_USB_PATH is ('${PICO_PIO_USB_PATH}')")
endif()

#
# For TinyUSB
#
set(FAMILY rp2040)

#
# Include pico_sdk_import.cmake from pico-sdk (instead of copying)
#
include(${PICO_SDK_PATH}/external/pico_sdk_import.cmake)

#
# Generic setup
#
set(PROJECT minimal-pico-tinyusb-pio-project)
project(${PROJECT})

#
# Initialize Pico-SDK
#
pico_sdk_init()

#
# Setup paths to Pico-PIO-USB
#
set(PICO_PIO_USB_SRC "${PICO_PIO_USB_PATH}/src")
set(PICO_PIO_USB_EXAMPLES_PATH ${PICO_PIO_USB_PATH}/examples)

#
# Set the path to the source code to build
#
if (NOT SRC_TO_BUILD_PATH)
    set(SRC_TO_BUILD_PATH ${PICO_PIO_USB_EXAMPLES_PATH}/host_hid_to_device_cdc)
endif()

message("Building code from '${SRC_TO_BUILD_PATH}'")
add_subdirectory(${SRC_TO_BUILD_PATH})
