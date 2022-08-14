# Minimal example project for using TinyUSB with Pico-PIO-USB on a Raspberry Pi Pico
TinyUSB with Pico-PIO-USB can do bitbanged USB hosts/device on a Raspberry Pi Pico

The purpose of this repo is demonstrating a simple way to get the example code to compile as a separate project so check the CMakeLists.txt file.

The actual source code that is build isn't a part of this repo but comes from https://github.com/sekigon-gonnoc/Pico-PIO-USB.git (it is included in tinyusb https://github.com/hathach/tinyusb as a submodule under /hw/mcu/raspberry_pi/Pico-PIO-USB)

It isn't necessary to checkout tinyusb and pico-sdk as submodules if you already have them available, see below for details.

## Requirements
### Tools
- cmake
- arm-none-eabi-gcc

### External Libraries (Included as git submodules for convenience)
- tinyusb
- pico-sdk

## Quickstart 
```bash
# Checkout tinyusb & pico-sdk as git submodules
git submodule update --init -- tinyusb pico-sdk

# Checkout Pico-PIO-USB as git submodules in tinyusb
git -C tinyusb submodule update --init hw/mcu/raspberry_pi/Pico-PIO-USB

# Build
cmake -B _build && cmake --build _build/ --config Release

# Done
ls _build/*.u2f
```

## Extras

### Alternate variants

#### Copying the example code to ./src and build it there
```bash
cp -R tinyusb/hw/mcu/raspberry_pi/Pico-PIO-USB/examples/host_hid_to_device_cdc ./src/
```
Then in ./src/host_hid_to_device_cdc/CMakeLists.txt delete the line
```cmake
set(PICO_PIO_USB_SRC "${CMAKE_CURRENT_LIST_DIR}/../../src")
```
Build like usual:
```bash
cmake -B _build && cmake --build _build/ --config Release
ls _build/*.u2f
```

#### Overriding paths to pico-sdk and tinyusb
```bash
cmake -B _build -DPICO_SDK_PATH=$PWD/pico-sdk  -DPICO_TINYUSB_PATH=$PWD/tinyusb && cmake --build _build/ --config Release
```

#### Use branch "develop" of pico-sdk:
```bash
git submodule add -b develop https://github.com/raspberrypi/pico-sdk pico-sdk
```

#### GIT Submodule commands cheatsheet
```bash
# Checkout code
git submodule update --init -- tinyusb pico-sdk

# Checkout one submodule in a submodule
git -C tinyusb submodule update --init hw/mcu/raspberry_pi/Pico-PIO-USB

# Check status
git submodule status --recursive
```

### Setting up your own project like this

#### Create empty git repo
```bash
git init barbaz
cd barbaz
mkdir src
```

#### Add tinyusb and pico-sdk as submodules
```bash
git submodule add https://github.com/hathach/tinyusb tinyusb
git submodule add https://github.com/raspberrypi/pico-sdk pico-sdk
```

#### Initialize the git submodule for Pico-PIO-USB included in tinyusb
```bash
git -C tinyusb submodule update --init hw/mcu/raspberry_pi/Pico-PIO-USB
```

#### Copy CMakeLists.txt from this repo
```bash
cp ../minimal-pico-tinyusb-pio-project/CMakeLists.txt ./
```

#### Build
```bash
cmake -B _build
cmake --build _build/ --config Release
```
