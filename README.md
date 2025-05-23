# Code Repository <br> Simplified Embedded Rust: STM32 Blog Collection Edition 🦀 

<p align="center">
<img src="https://media.beehiiv.com/cdn-cgi/image/fit=scale-down,format=auto,onerror=redirect,quality=80/uploads/asset/file/c871dc93-7b9d-4fe5-bf11-608dd8c08949/Book_Cover_Temp_A3.png?t=1747850029" alt="BookCover" width="200"/>
</p>

Welcome to the _**Simplified Embedded Rust: STM32 Blog Collection Edition**_ examples repository. Here you will find all the resources related to the book & the blog.

This repository contains example projects written in Rust for the STM32 Nucleo-F401RE development board from ST Microelectronics. The various projects can be used as a starting point for programming your Nucleo board.

Each of these these projects has an accompanying blog post explaining the code in more detail [here](https://blog.theembeddedrustacean.com/). 

Alternatively, for a self-contained resource, you can get a free copy of the eBook by subscribing The Embedded Rustacean Newsletter [here](https://www.theembeddedrustacean.com/subscribe).


## 📝 Reporting & Correcting Issues
If you find any code issues in the book, please submit an issue or a pull request with the correction. 

## 🧑‍💻 Environment Setup

Each of the examples is its own standalone project. The examples are designed to be run on the STM32 Nucleo-F401RE development board, however, they can be modified to run on other STM32 devices as well.
The projects can be used as templates and were generated in VSCode.

The code in this repository should also be compatible with a various number of STM32 devices that are supported by the STM32F4xx HAL. You can refer to the STM32F4xx HAL repository for the full list of devices that are supported.

## 1. 🌐 Clone Repository and Install Tools (Recommended)
This is the quickest and easiest way to get started. The projects in the repo are already configured to run on the STM32 Nucleo-F401RE development board. You can clone the repository and run the projects after installing compile & debug tools. This entails installing the following tools:

- The Rust Compile Toolchain (including cross-compilation tools)
- The ARM Debug Toolchain (GDB + OpenOCD) - The STM32F4 is an ARM-based device

Here are the steps to install the tools:

## 🏗️ Compile Toolchain

### Rustup Installation

Install rustup by following the instructions at [https://rustup.rs](https://rustup.rs/).

If you already have rustup installed double check that you are on the stable channel and your stable toolchain is up to date. `rustc -V` should return a date newer than the one shown below:

```bash
$ rustc -V
rustc 1.31.0 (abe02cefd 2018-12-04)
```

The default installation of Rust only supports native compilation of the host machine. Therefore, you'd need to add a target that can cross-compile for the STM32F4. To add the ARM Cortex-M4 with a FPU (processor incorporated in the STM32F4) as a targer the command is:

```bash
$ rustup target add thumbv7em-none-eabihf
```

## ⚡️ Flash & Debug Toolchain

### Other OS Specific Instructions

- [Windows](https://docs.rust-embedded.org/book/intro/install/windows.html)
- [Linux](https://docs.rust-embedded.org/book/intro/install/linux.html)

### Install GDB (macOS Specific)

GDB is the GNU Debugger. It is a powerful tool that allows you to debug your code. To install GDB, run the following command in a terminal window:

```bash
brew install arm-none-eabi-gcc
```

### Install OpenOCD (macOS Specific)

OpenOCD (Open On-Chip Debugger) is an open-source tool that allows you to flash and debug your code. It provides a GDB server interface that allows you to connect to your target device and debug your code. OpenOCD supports a variety of hardware interfaces, including JTAG and SWD. To install OpenOCD, run the following command in a terminal window:

```bash
$ brew install openocd
```

### Test Board Connection

After installing the tools you can test the connection with your board. To do that, connect your STM32 board, and run the following command:

```bash
openocd -f interface/stlink.cfg -f target/stm32f4x.cfg
```

> ⚠️ Note that the `-f` flag is used to specify the configuration files for OpenOCD. The `interface/stlink.cfg` file specifies the ST-Link interface, and the `target/stm32f4x.cfg` file specifies the STM32F4 target. If you are using a different STM32 device, you'd have to change the configuration.

Once running the command, you should get a result similar to the following:

```bash
Open On-Chip Debugger 0.10.0
Licensed under GNU GPL v2
For bug reports, read
    http://openocd.org/doc/doxygen/bugs.html
Info : auto-selecting first available session transport "hla_swd". To override use 'transport select <transport>'.
adapter speed: 1000 kHz
adapter_nsrst_delay: 100
Info : The selected transport took over low-level target control. The results might differ compared to plain JTAG/SWD
none separate
Info : Unable to match requested speed 1000 kHz, using 950 kHz
Info : Unable to match requested speed 1000 kHz, using 950 kHz
Info : clock speed 950 kHz
Info : STLINK v2 JTAG v27 API v2 SWIM v15 VID 0x0483 PID 0x374B
Info : using stlink api v2
Info : Target voltage: 2.900414
Info : stm32f4x.cpu: hardware has 6 breakpoints, 4 watchpoints
```

## 2. 🛠️ Configure your Own Project in VSCode   
This option is for those who want to set up their own project from scratch. This option is recommended for those who are already familiar with the STM32 development environment and want to customize their project. For example, you may want to use a different STM32 device or a different development board.

You still need to install the tools mentioned in the previous section. After that, you can create your own project using the following steps:

## Install the Cortex-Debug Extension

If not already installed, install [Cortex-Debug Extension](https://marketplace.visualstudio.com/items?itemName=marus25.cortex-debug) in VSCode.

## Install the Rust Analyzer Extension

If not already installed, install [Rust-Analyzer Extension](https://marketplace.visualstudio.com/items?itemName=rust-lang.rust-analyzer) in VSCode.

## Generate Standard Project Template

You can use cargo generate to obtain standard template or you can create your own. Even if you generate from the template then you need to access the files that are mentioned later to update each accordingly. To install cargo generate, run the following command:

```bash
cargo install cargo-generate 
```

Then, you can generate a new project using a template through the following command:

```bash
cargo generate --git https://github.com/rust-embedded/cortex-m-quickstart
```

## Modify/Verify openocd.cfg

In the newly generated project template, go to the `openocd.cfg` file in the project directory and make sure the Open OCD configuration file is compatible with your STM32 as follows:

```
source [find interface/stlink.cfg]
source [find target/stm32f4x.cfg]
```

## Modify/Verify openocd.gdb

You can modify the openocd.gdb file in the project directory as well. This file contains the command script used to automate the setup of a debugging session with OpenOCD. You can keep the minimal file but this is also a recommended configuration:

```
target extended-remote :3333
set print asm-demangle on
set print pretty on
load
monitor arm semihosting enable
continue
```

## Modify/Verify .cargo/config.toml

Before we run any code, we need to configure the project so that it compiles for the correct target and provide linker script arguments. For example, the `thumbv7m-none-eabihf` is the compile target in the STM32F4 case.  We do this by creating/modifying the file `.cargo/config` in the project root. A minimal file would look as follows:

```bash
[target.thumbv7em-none-eabihf]
runner = "arm-none-eabi-gdb -q -x openocd.gdb"
rustflags = [
  "-C", "link-arg=-Tlink.x",
]

[build]
target = "thumbv7em-none-eabihf"
```

`runner = "arm-none-eabi-gdb -q -x openocd.gdb"` uses GDB with OpenOCD to flash and debug the target automatically. `rustflags = [ "-C", "link-arg=-Tlink.x" ]` passes the link.x linker script. Finally, `target = "thumbv7em-none-eabihf"` sets the default compilation target to ARM Cortex-M with hardware float.`

## Modify/Verify Linker Script

The linker script tells the linker about the memory layout of the chip used. These values are obtained from the datasheet of the device. You can find this information in the `memory.x` file in the project root. For example, for the STM32F401RE the file looks as follows:

```bash
MEMORY
{
  /* NOTE 1 K = 1 KiBi = 1024 bytes */
  FLASH : ORIGIN = 0x08000000, LENGTH = 512K
  RAM : ORIGIN = 0x20000000, LENGTH = 96K
}
```

## Modify/Verify Dependencies in Cargo.toml

The `Cargo.toml` file is used to configure your project. There is already one present in the project folder, but you can add some dependencies and configurations as shown below. This is also for the STM32F4 as you might notice. Make sure you replace the `{{authors}}` and `{{project-name}}` fields with the author(s) and the project file name, respectively.

```bash
#Cargo.toml
[package]
authors = ["{{authors}}"]
edition = "2018"
readme = "README.md"
name = "{{project-name}}"
version = "0.1.0"

[dependencies]
cortex-m = "0.6.0"
cortex-m-rt = "0.7.1"
cortex-m-semihosting = "0.3.3"
panic-halt = "0.2.0"
embedded-hal = "0.2.7"
rtt-target = { version = "0.3.1", features = ["cortex-m"] }

# Uncomment for the panic example.
# panic-itm = "0.4.1"

# Uncomment for the allocator example.
# alloc-cortex-m = "0.4.0"

[dependencies.stm32f4xx-hal]
version = "0.13.1"
features = ["rt", "stm32f401"] # replace the model of your microcontroller here

[[bin]]
name = "{{project-name}}"
test = false
bench = false

[profile.release]
codegen-units = 1 # better optimizations
debug = true # symbols are nice and they don't increase the size on Flash
lto = true # better optimizations
```

## Modify/Verify .vscode/launch.json

To define the debug launch properties, there should be already a `.vscode/launch.json` in your project root that you can modify. Again, make sure you replace the `{{project-name}}` field with the project file name. If created manually, minimal file would look as follows:

```json
{
    "version": "0.2.0",
    "configurations": [
        {
            "type": "cortex-debug",
            "request": "launch",
            "name": "Debug (OpenOCD)",
            "servertype": "openocd",
            "cwd": "${workspaceRoot}",
            "executable": "./target/thumbv7em-none-eabihf/debug/{{project-name}}",
            "preLaunchTask": "build",
            "device": "STM32F401RET6U",
            "configFiles": [
                "interface/stlink.cfg",
                "target/stm32f4x.cfg"
            ],
            "svdFile": "${workspaceRoot}/STM32F401.svd",
            "runToEntryPoint": "main",
        }
    ]
}
```

## Modify/Verify .vscode/tasks.json

The `tasks.json` defines the build tasks that need to be performed with each build in VSCode. A standard template is provided with cargo-generate. If created manually, minimal file would look as follows:

```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "label": "build",
            "type": "process",
            "command": "cargo",
            "args": [
                "build"
            ],
            "problemMatcher": [
                "$rustc"
            ],
            "group": {
                "kind": "build",
                "isDefault": true
            }
        }
    ]
}
```
