# Minimal example project for using TinyUSB with Pico-PIO-USB on a Raspberry Pi Pico
TinyUSB with Pico-PIO-USB can do bitbanged USB hosts/device on a Raspberry Pi Pico

The purpose of this repo is demonstrating a simple way to get the example code to compile as a separate project so check the CMakeLists.txt file.

The actual source code that is built isn't a part of this repo but comes from https://github.com/sekigon-gonnoc/Pico-PIO-USB.git (it is included in TinyUSB under `/hw/mcu/raspberry_pi/Pico-PIO-USB`). The project now relies on the TinyUSB copy shipped inside pico-sdk 2.2.0, so no separate TinyUSB submodule is required.

## Requirements
### Tools
- cmake
- arm-none-eabi-gcc

### External Libraries (included as git submodules for convenience)
- pico-sdk

## Quickstart 
```bash
# Checkout pico-sdk (includes TinyUSB)
git submodule update --init --recursive pico-sdk

# Build
cmake -B _build && cmake --build _build/ --config Release

# Done
find _build/ -type f -name "*.uf2" -ls
```

## Extras

### Alternate variants

To build a different TinyUSB example, point `SRC_TO_BUILD_PATH` at the directory inside `pico-sdk/lib/tinyusb/examples`. For example:
```bash
cmake -B _build -DSRC_TO_BUILD_PATH=$PWD/pico-sdk/lib/tinyusb/examples/device/cdc_msc && cmake --build _build/ --config Release
find _build/ -type f -name "*.uf2" -ls
```

#### Overriding paths to pico-sdk and tinyusb
```bash
cmake -B _build -DPICO_SDK_PATH=$PWD/pico-sdk  -DPICO_TINYUSB_PATH=$PWD/pico-sdk/lib/tinyusb && cmake --build _build/ --config Release
```

#### Use branch "develop" of pico-sdk:
```bash
git submodule add -b develop https://github.com/raspberrypi/pico-sdk pico-sdk
```

#### GIT Submodule commands cheatsheet
```bash
# Checkout code
git submodule update --init --recursive pico-sdk

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

#### Add pico-sdk as a submodule
```bash
git submodule add https://github.com/raspberrypi/pico-sdk pico-sdk
git submodule update --init --recursive pico-sdk
```

#### Copy CMakeLists.txt from this repo
```bash
cp ../minimal-pico-tinyusb-pio-project/CMakeLists.txt ./
```

#### Build
```bash
cmake -B _build
cmake --build _build/ --config Release
find _build/ -type f -name "*.uf2" -ls
```
