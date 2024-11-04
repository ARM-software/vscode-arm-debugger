# Arm Debugger

## Overview

The complete documentation for [Arm® Debugger](https://developer.arm.com/documentation/108029/latest/Arm-Debugger-extension) and the other [Keil® Studio extensions](https://developer.arm.com/documentation/108029/latest/Extension-pack-and-extensions) is available on Arm Developer.

The Arm Debugger extension provides access to the Arm Debugger engine for Visual Studio Code by implementing the [Microsoft Debug Adapter Protocol (DAP)](https://microsoft.github.io/debug-adapter-protocol//). The Arm Debugger engine supports connections to physical and virtual targets:

- Physical targets. Either through external debug probes such as the Arm's ULINK™ family of debug probes, or through on-board low-cost debugging such as ST-Link or CMSIS-DAP based debug probes.
- Virtual targets. Using Fixed Virtual Platform models (FVPs).

See the [Arm Debugger Release Note](https://developer.arm.com/documentation/109667/latest) for an overview of each release.

We recommend installing the [Keil Studio Pack](https://marketplace.visualstudio.com/items?itemName=Arm.keil-studio-pack) for Visual Studio Code Desktop to quickly set up your environment. You can then [import a solution example from keil.arm.com](https://developer.arm.com/documentation/108029/latest/Get-started-with-an-example-project/Import-a-solution-example), [download a μVision project from keil.arm.com](https://developer.arm.com/documentation/108029/latest/Get-started-with-an-example-project/Download-a-Keil--Vision-example), [create a solution from scratch](https://developer.arm.com/documentation/108029/latest/Arm-CMSIS-Solution-extension/Create-a-solution), or [convert an existing μVision project](https://developer.arm.com/documentation/108029/latest/Arm-CMSIS-Solution-extension/Convert-a-Keil--Vision-project-to-a-solution).

The Arm Debugger extension works with the Arm CMSIS Solution (Identifier: `arm.cmsis-csolution`), the Arm Device Manager (Identifier: `arm.device-manager`), and the Arm Virtual Hardware (Identifier: `arm.virtual-hardware`) extensions.

## Supported features

The Arm Debugger extension allows you to:

- Load images (for example, .elf, .axf, or .bin files) and debug information as per the DWARF Debugging Information Format Standard, up to and including version 5
- Run images
- Set breakpoints
- Do source and instruction stepping
- Access variables and registers
- View the content of memory
- Debug FVPs
- Access the [CLI](https://developer.arm.com/documentation/101471/2023-0/Arm-Debugger-commands) using the Debug Console

## Debug configuration support

The Arm Debugger engine supports debug connections based on:

- The [debug setup with CMSIS-Pack](https://open-cmsis-pack.github.io/Open-CMSIS-Pack-Spec/main/html/coresight_setup.html)
- The integrated [configuration database (configdb)](https://developer.arm.com/documentation/101470/2023-0/DTSL/Arm-Development-Studio-configuration-database)

## Supported architectures and processors

- Armv6-M architecture

  Cortex®-M0, Cortex-M0+, Cortex-M1, Arm SecurCore® SC000

- Armv7-M architecture

  Cortex-M3, Cortex-M4, Cortex-M7, Arm SecurCore SC300

- Armv8-M architecture

  Cortex-M23, Cortex-M33, Cortex-M35P

- Armv8.1-M architecture

  Cortex-M55, Cortex-M85

## Supported debug probes

- [Arm ULINK family](https://www.arm.com/products/development-tools/debug-probes/ulink)
- ST-LINK from STMicroelectronics
- Probes based on the [CMSIS-DAP](https://arm-software.github.io/CMSIS_5/latest/DAP/html/index.html) debug adapter protocol v1.x and v2.x

## Limitations

Some capabilities of the Arm Debugger engine that are not yet exposed in Visual Studio Code include:

- CoreSight™ trace
- Symmetric and asymmetric multicore debug
- RTOS awareness

## Use the Arm Debugger extension

See the [documentation](https://developer.arm.com/documentation/108029/latest/Arm-Debugger-extension) for the Arm Debugger extension to run a project on your hardware and start a debug session.

## Pre-releases

The Arm Debugger extension introduces pre-releases from version 1.3.0. It follows the recommended [versioning](https://code.visualstudio.com/api/working-with-extensions/publishing-extension#prerelease-extensions) scheme `Major.Minor.Patch` with:
- Arm Debugger extension release: `Minor` is an even number, for example `1.2.0`.
- Arm Debugger extension pre-release: `Minor` is an odd number, for example `1.3.0`.

## Submit feedback or report issues

To submit feedback or report issues on the Arm Debugger extension, please use [GitHub issues](https://github.com/Arm-Software/vscode-arm-debugger/issues/new/choose) in the extension repository.
