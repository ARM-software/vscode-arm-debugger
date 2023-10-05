# Arm Debugger

## Overview

The **Arm Debugger** extension provides access to the Arm Debugger for Visual Studio Code, implementing the Microsoft Debug Adapter Protocol. Arm Debugger supports connections to physical targets, either via external debug probes such as Arm's ULINK™ family of debug probes, or via on-board low-cost debugging such as CMSIS-DAP based debug probes.

### Arm Debugger supports
- Loading images (e.g. .elf, .axf, .bin) and debug information as per the DWARF debug information standard up to and including version 5
- Running images
- Setting breakpoints
- Source and instruction stepping
- Accessing variables and registers
- Viewing the content of memory
- Access to [CLI](https://developer.arm.com/documentation/101471/2023-0/Arm-Debugger-commands) via Debug Console

### Debug Configuration Support
The Arm Debugger for Visual Studio Code supports debug connections based on:

- [CMSIS-Pack Debug Setup](https://open-cmsis-pack.github.io/Open-CMSIS-Pack-Spec/main/html/coresight_setup.html)
- The integrated [Debug Configuration Database](https://developer.arm.com/documentation/101470/2023-0/DTSL/Arm-Development-Studio-configuration-database)

### Supported Processor and Architecture
- Armv6-M Architecture
    - Cortex-M0, Cortex-M0+, Cortex-M1, Arm SecurCore™ SC000
- Armv7-M Architecture
    - Cortex-M3, Cortex-M4, Cortex-M7, Arm SecurCore™ SC300
- Armv8-M Architecture
    - Cortex-M23, Cortex-M33, Cortex-M35P
- Armv8.1-M Architecture
    - Cortex-M55, Cortex-M85

### Supported debug probes
- [Arm ULINK family](https://www.arm.com/products/development-tools/debug-probes/ulink)
- ST Microelectronics ST-Link
- Probes based on the [CMSIS-DAP](https://arm-software.github.io/CMSIS_5/latest/DAP/html/index.html) debug adapter protocol v1.x and v2.x

## Status

The Arm Debugger extension is currently a preview, with a limited subset of debug features exposed in the VS Code user interface. Additional features are available through the command line. At this stage, we recommend using Arm Debugger through the [Keil Studio Extension Pack pre-release channel](https://marketplace.visualstudio.com/items?itemName=Arm.keil-studio-pack) because the extension pack provides an easy way to get started. Using Arm Debugger as a standalone extension is possible and requires additional configuration.

Capabilities of Arm Debugger that are not yet exposed in Visual Studio Code include:
- CoreSight trace
- Symmetric and asymmetric multicore debug
- RTOS awareness

## Install and get started with the Arm Debugger extension and dependencies

1. We recommend installing Arm Debugger as part of the [Keil Studio extension pack](https://marketplace.visualstudio.com/items?itemName=Arm.keil-studio-pack). The Arm Debugger extension is included as a dependency.

1. Switch to the **pre-release channel** of the Keil Studio extension.

1. Clone one of the project from [Hello World Example projects](https://github.com/Arm-Examples#hello-world-examples). The example contains a `vcpkg-configuration.json` manifest file, which describes the required dependencies of the example project.

1. Install Arm Debugger by adding following entry to `vcpkg-configuration.json` manifest file in `requires` section:
    ```"arm:debuggers/arm/armdbg": "^6.0.0"```

1. The **Arm Environment Manager** will automatically use the `vcpkg-configuration.json` file to locate and install the required tools dependencies for the example project, including the Arm Debugger engine. Tools dependencies are made available on the path.


## Use of Arm Debugger

1. Build the project by opening `CMSIS` panel and click `Build` button. Optionally you can run `CMSIS Build` task from `Tasks: Run Task` in `Command Palette (CTRL+Shift+P)`.

1. Connect your board and select it in [Device Manager](https://marketplace.visualstudio.com/items?itemName=Arm.device-manager) panel. This extension is installed as part of [Keil Studio extension pack](https://marketplace.visualstudio.com/items?itemName=Arm.keil-studio-pack).

1. Create a flash task in `.vscode/tasks.json` using `Command Palette (CTRL+Shift+P)` -> `Tasks: Configure Task` -> `arm-debugger.flash: Flash Device`. It will create following entry:
    ```
    {
		"type": "arm-debugger.flash",
		"serialNumber": "${command:device-manager.getSerialNumber}",
		"program": "${command:arm-debugger.getApplicationFile}",
		"cmsisPack": "${command:device-manager.getDevicePack}",
		"problemMatcher": [],
		"label": "arm-debugger.flash: Flash Device"
	}
    ```

1. Deploy the application binary on the board by running the flash task: `Command Palette (CTRL+Shift+P)` -> `Tasks: Run Task` -> `Flash Device using Arm Debugger` and select correct device name and CPU when prompted.

1. To debug a program on the board create an `Arm Debug` configuration in `.vscode/launch.json` file. A file can be created by going into `Run and Debug` panel and selecting `create a launch.json file` -> `ARM Debugger` option. It will create following entry:
    ```
    {
		"name": "Arm Debug",
		"type": "arm-debug",
        "request": "launch",
		"serialNumber": "${command:device-manager.getSerialNumber}",
		"program": "${command:embedded-debug.getApplicationFile}",
		"cmsisPack": "${command:device-manager.getDevicePack}"
	}
    ```

1. To start a debug session open `Run & Debug (ctrl+Shift+D)` panel and select `Arm Debug` configuration.

## Configuring Arm Debugger

Visual Studio Code uses launch configuration files to provide settings to the debugger. Example `.vscode/launch.json` file:

```
{
        "configurations": [
        {
            "name": "Arm Debug",
            "type": "arm-debug",
            "request": "launch",
            "serialNumber": "${command:device-manager.getSerialNumber}",
            "program": "${command:embedded-debug.getApplicationFile}",
            "cmsisPack": "${command:device-manager.getDevicePack}",
            "connectMode": "auto",
            "debugFrom": "main",
            "probe": "CMSIS-DAP",
            "programMode": "auto",
            "resetMode": "auto"
        }    
    ]
}
```

## Submit feedback

To submit feedback while the Arm Debugger extension is a preview, please [open a bug report or a feature request](https://github.com/Arm-Software/vscode-arm-debugger/issues/new/choose).
