# Arm Debugger

- [Overview](#overview)
- [Install the extension](#install-the-extension)
- [Install the Arm Debugger engine and other tools with vcpkg](#install-the-arm-debugger-engine-and-other-tools-with-vcpkg)
- [Define a run configuration](#define-a-run-configuration)
- [Run the project on your board](#run-the-project-on-your-board)
- [Define a debug configuration](#define-a-debug-configuration)
- [Start a debug session](#start-a-debug-session)
- [Pre-releases](#pre-releases)
- [Limitations](#limitations)
- [Submit feedback or report issues](#submit-feedback-or-report-issues)

## Overview

The Arm® Debugger extension allows you to debug and optimize software running on Arm-based processors.

You can install this extension individually or as part of the [Arm Keil® Studio Pack](https://marketplace.visualstudio.com/items?itemName=Arm.keil-studio-pack).

This README explains how to use the standalone extension. To use the Keil Studio Pack, check [Get started with an example project](https://developer.arm.com/documentation/108029/latest/Get-started-with-an-example-project) in the Arm Keil Studio Visual Studio Code Extensions User Guide on the [Arm Developer website](https://developer.arm.com). The complete [documentation](https://developer.arm.com/documentation/108029/latest/Arm-Debugger-extension) for Arm Debugger is available on Arm Developer.

The Arm Debugger extension provides access to the Arm Debugger engine by implementing the [Microsoft Debug Adapter Protocol (DAP)](https://microsoft.github.io/debug-adapter-protocol//).

The Arm Debugger engine supports connections to the following targets:

- Physical targets: Either through external debugging, for example, Arm ULINK™ debug probes, or through on-board low-cost debugging, for example, ST-Link or CMSIS-DAP based debug probes.

- Virtual targets: Using Fixed Virtual Platform models (FVPs).

See the [Arm Debugger Release Note](https://developer.arm.com/documentation/109667/latest) for an overview of each Arm Debugger engine release.

### Supported features

The Arm Debugger extension allows you to:

- Load images like .elf, .axf, or .bin files and debug information according to the DWARF Debugging Information Format Standard, up to and including version 5

- Run images

- Set breakpoints

- Do source and instruction stepping

- Access variables and registers

- View the content of memory

- Debug FVPs

- Access the [CLI](https://developer.arm.com/documentation/101471/2023-0/Arm-Debugger-commands) using the Debug Console

### Debug connection support

The Arm Debugger engine supports debug connections based on the following information:

- The [debug setup included in the CMSIS-Packs](https://open-cmsis-pack.github.io/Open-CMSIS-Pack-Spec/main/html/coresight_setup.html) of your project

- The integrated [configuration database (configdb)](https://developer.arm.com/documentation/101469/latest/Introduction-to-Arm-Debugger/Debugger-concepts). The configuration database is where Arm Debugger stores information about the processors, devices, and boards that it can connect to.

### Supported architectures and processors

- Armv6-M architecture

  Cortex®-M0, Cortex-M0+, Cortex-M1, Arm SecurCore® SC000

- Armv7-M architecture

  Cortex-M3, Cortex-M4, Cortex-M7, Arm SecurCore SC300

- Armv8-M architecture

  Cortex-M23, Cortex-M33, Cortex-M35P

- Armv8.1-M architecture

  Cortex-M55, Cortex-M85

### Supported debug probes

- [Arm ULINK family](https://www.arm.com/products/development-tools/debug-probes/ulink)

- ST-LINK from STMicroelectronics

- Probes based on the [CMSIS-DAP](https://arm-software.github.io/CMSIS-DAP/latest/) debug adapter protocol v1.x and v2.x

## Install the extension

1. Open Visual Studio Code Desktop and click the **Extensions** icon ![Extensions icon](https://github.com/ARM-software/vscode-arm-debugger/raw/main/docs/images/extensions-icon.png) in the Activity Bar to open the **Extensions** view.

1. Search for **Arm Debugger** (identifier: `arm.arm-debugger`) and click **Install**.

    The Arm Tools Environment Manager extension is installed at the same time.

## Install the Arm Debugger engine and other tools with vcpkg

The Arm Tools Environment Manager extension downloads, installs, and manages software development tools, including the Arm Debugger engine, using [Microsoft vcpkg](https://vcpkg.io/en/index.html) artifacts. Your project includes a vcpkg-configuration.json manifest file. Arm Tools Environment Manager uses this file to acquire and activate the tools needed to set up your development environment.

The [Arm Tools Artifactory](https://www.keil.arm.com/artifacts/) lists the tools (compiler, debugger, simulation models, and utilities) and the different versions available. Some tools, like the Arm Debugger engine, require a [user-based licensing (UBL)](https://developer.arm.com//Tools%20and%20Software/User-based%20Licensing) license. You must activate the license with the **Arm License Management Utility** available with Arm Tools Environment Manager.

See the [Arm Tools Environment Manager README](https://marketplace.visualstudio.com/items?itemName=Arm.environment-manager) for more details.

To install the Arm Debugger engine and other tools:

1. Click the **Arm Tools** status bar item and select **Add Arm Tools Configuration to Workspace** in the drop-down list at the top of the window. Alternatively, right-click in the Explorer and select **Configure Arm Tools Environment**.

    A visual editor opens where you can select an Arm Debugger version and the other tools you need.

1. Select the latest version available for **Arm Debugger**.

    The options that you select in the editor are added in a `vcpkg-configuration.json` file. You can also update the `vcpkg-configuration.json` file manually.

    For example, the following file activates the Arm Debugger engine.

    ```json
    {
        "registries": [
            {
                "name": "arm",
                "kind": "artifact",
                "location": "https://artifacts.tools.arm.com/vcpkg-registry"
            }
        ],
        "requires": {
            "arm:debuggers/arm/armdbg": "6.3.0"
        }
    }
    ```

1. Save your changes.

    A dialog box opens where you can choose where to save the `vcpkg-configuration.json` file.

1. Save the `vcpkg-configuration.json` file at the root of your workspace.

    The Arm Tools Environment Manager extension activates the workspace and downloads the tools specified in the `vcpkg-configuration.json` file. After installation, tools are available on the PATH.

1. Move your mouse over the **Arm Tools** status bar item to display information about the tools installed.

    **Note**: Alternatively, you can start from one of the official examples available on [Arm-Examples](https://github.com/Arm-Examples) or [keil.arm.com](https://www.keil.arm.com/boards/). The official examples contain pre-configured `vcpkg-configuration.json` files.

1. The status bar shows **No Arm License** if no valid UBL license is active. In this case, click **No Arm License** and select **Activate or manage Arm licenses** in the drop-down list at the top of the window.

1. Manage your license with the **Arm License Management Utility**.

    ![Status bar](https://github.com/ARM-software/vscode-arm-debugger/raw/main/docs/images/status-bar.png)

## Define a run configuration

To download your code and run it on your hardware, you must first configure a task. Visual Studio Code uses a `tasks.json` configuration file to run projects. Use the **Run Configuration** visual editor to select options. Alternatively, you can add the configuration manually in the `tasks.json` file.

To define a run configuration:

1. Go to the **Terminal** menu and select **Configure Tasks**, then select the `arm-debugger.flash: Flash Device` task in the drop-down list at the top of the window.

    Alternatively, you can go to the **View** menu and select **Command Palette**, then select **Tasks: Configure Task**.

    Visual Studio Code creates a `tasks.json` file in the `.vscode` folder of your project with the following default configuration:

    ```json
    {
        "version": "2.0.0",
        "tasks": [
            {
                "type": "arm-debugger.flash",
                "serialNumber": "<serial number of your device>",
                "program": "${command:arm-debugger.getApplicationFile}",
                "cmsisPack": "<path or URL of CMSIS Pack for your device>",
                "problemMatcher": [],
                "label": "arm-debugger.flash: Flash Device"
            }
        ]
    }
    ```

1. To open the **Run Configuration** visual editor, click **Open Arm Run Configuration** ![Run Configuration icon](https://github.com/ARM-software/vscode-arm-debugger/raw/main/docs/images/run-debug-config-icon.png) in the top right-hand corner of the text editor.

    ![Run Configuration editor](https://github.com/ARM-software/vscode-arm-debugger/raw/main/docs/images/run-config-editor.png)

1. Use the visual editor to enter the following details:

    - **Probe/Unit** section:

        - **Serial Number**: Type the serial number of your device.

        **Note:** If you have the Arm Device Manager extension installed, then connect your device over USB and select `auto` in the drop-down list. With `auto`, the Arm Debugger extension uses the serial number of the active device in the Arm Device Manager extension by default.

    - **Target** section:

        - **CMSIS-Pack**: Use the short code for the Device Family Pack (DFP) for the target debug probe or board in the format `<vendor>::<pack>@<version>`. For example: `ARM::Cortex_DFP@1.1.0`. You can also type the path or URL to the DFP.

        - **CMSIS-Pack Device Name**: Type the name of the target device, that is, the target chip on your board.

        - **Processor Name**: For multicore devices, type the name of the processor to use.

    - **Application** section:

        - **Program Files**: Click **Add File** to point to an AXF or ELF file. You can add as many files as you need. The Arm Debugger extension uses the files in the order in which you added them. You can also use `${command:arm-debugger.getApplicationFile}`. This command detects the AXF or ELF file to use or offers a selection of files available if several AXF or ELF files are present in your workspace.

1. Save your changes.

    The options that you select in the editor are added in the `tasks.json` file.

For information on more options, see [Run configuration options in the visual editor](https://developer.arm.com/documentation/108029/latest/Arm-Debugger-extension/Run-your-project-on-your-hardware-with-Arm-Debugger/Modify-the-run-configuration-options-with-the-Run-Configuration-visual-editor/Run-configuration-options-in-the-visual-editor) and [Arm Debugger run configuration options](https://developer.arm.com/documentation/108029/latest/Arm-Debugger-extension/Run-your-project-on-your-hardware-with-Arm-Debugger/Arm-Debugger-run-configuration-options).

**Note**: Alternatively, you can start from one of the official examples available on [Arm-Examples](https://github.com/Arm-Examples) or [keil.arm.com](https://www.keil.arm.com/boards/). The official examples contain pre-configured `tasks.json` files.

## Run the project on your board

Download your code and run it on your hardware.

**Note**: In this procedure, it is assumed that you have already built an AXF or ELF file with your toolchain.

1. Go to the **View** menu and select **Command Palette**, then select **Tasks: Run Task** and select the `arm-debugger.flash: Flash Device` task in the drop-down list at the top of the window. Alternatively, go to the **Terminal** menu and select **Run Task**, then select the task.

1. When you run a task for the first time, Visual Studio Code asks you to select a scanning option for the task output. Select the `Continue without scanning the task output` option in the drop-down list.

    If you are using a multicore device and you did not specify a `"processorName"` in the `tasks.json` file, select the appropriate processor for your device in the **Select a processor** drop-down list at the top of the window.

    The project is downloaded to the board and starts running.

1. Check the download progress in the **Terminal** tab.

## Define a debug configuration

Visual Studio Code uses a `launch.json` configuration file to provide debug configuration options to the debugger. Use the **Debug Configuration** visual editor to select options. Alternatively, you can add the configuration manually in the `launch.json` file.

Several debug configurations are available:

- Physical targets: `Arm Debugger` adds a `Launch` configuration to launch your program in debug mode using a physical target.

- Virtual targets (Fixed Virtual Platforms or FVPs): `Arm Debugger FVP` adds a `Launch FVP` configuration to launch your program in debug mode using a virtual target.

### Debug configuration for a physical target

To define a debug configuration for a physical target:

1. Go to the **Run** menu and select **Add Configuration**, then select `Arm Debugger` in the drop-down list at the top of the window.

    Alternatively, you can go to the **View** menu and select **Command Palette**, then select **Debug: Add Configuration**.

    Visual Studio Code creates a `launch.json` file in the `.vscode` folder of your project with the following default configuration:

    ```json
    {
        // Use IntelliSense to learn about possible attributes.
        // Hover to view descriptions of existing attributes.
        // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
        "version": "0.2.0",
        "configurations": [
            {
                "name": "Arm Debugger",
                "type": "arm-debugger",
                "request": "launch",
                "serialNumber": "<serial number of your device>",
                "program": "${command:arm-debugger.getApplicationFile}",
                "cmsisPack": "<path or URL of CMSIS Pack for your device>"
            }
        ]
    }
    ```

1. To open the **Debug Configuration** visual editor, click **Open Arm Debug Configuration** ![Debug Configuration icon](https://github.com/ARM-software/vscode-arm-debugger/raw/main/docs/images/run-debug-config-icon.png) in the top right-hand corner of the text editor.

    ![Debug Configuration editor](https://github.com/ARM-software/vscode-arm-debugger/raw/main/docs/images/debug-config-editor.png)

1. Use the visual editor to enter the following details:

    - **Probe/Unit** section:

        - **Serial Number**: Type the serial number of your device.

        **Note:** If you have the Arm Device Manager extension installed, then connect your device over USB and select `auto` in the drop-down list. With `auto`, the Arm Debugger extension uses the serial number of the active device in the Arm Device Manager extension by default.

    - **Target** section:

        - **CMSIS-Pack**: Use the short code for the Device Family Pack (DFP) for the target debug probe or board in the format `<vendor>::<pack>@<version>`. For example: `ARM::Cortex_DFP@1.1.0`. You can also type the path or URL to the DFP.

        - **CMSIS-Pack Device Name**: Type the name of the target device, that is, the target chip on your board.

        - **Processor Name**: For multicore devices, type the name of the processor to use.

    - **Application** section:

        - **Program Files**: Click **Add File** to point to an AXF or ELF file. You can add as many files as you need. The Arm Debugger extension uses the files in the order in which you added them. You can also use `${command:arm-debugger.getApplicationFile}`. This command detects the AXF or ELF file to use or offers a selection of files available if several AXF or ELF files are present in your workspace.

1. Save your changes.

    The options that you select in the editor are added in the `launch.json` file.

For information on other options, see [Debug configuration options in the visual editor](https://developer.arm.com/documentation/108029/latest/Arm-Debugger-extension/Debug-your-project-with-Arm-Debugger/Modify-the-debug-configuration-options-with-the-Debug-Configuration-visual-editor/Debug-configuration-options-in-the-visual-editor) and [Arm Debugger debug configuration options](https://developer.arm.com/documentation/108029/latest/Arm-Debugger-extension/Debug-your-project-with-Arm-Debugger/Arm-Debugger-debug-configuration-options---CMSIS-use-cases).

**Note**: Alternatively, you can start from one of the official examples available on [Arm-Examples](https://github.com/Arm-Examples) or [keil.arm.com](https://www.keil.arm.com/boards/). The official examples contain pre-configured `launch.json` files.

### Debug configuration for a virtual target

To define a debug configuration for a virtual target:

1. To debug a virtual target using FVP models, you must install models on your machine.

    Add `"arm:models/arm/avh-fvp": "<version>"` in the `"requires":` section of the `vcpkg-configuration.json` file for your project.

1. Go to the **Run** menu and select **Add Configuration**, then select `Arm Debugger FVP` in the drop-down list at the top of the window.

    Alternatively, you can go to the **View** menu and select **Command Palette**, then select **Debug: Add Configuration**.

    Visual Studio Code creates a `launch.json` file in the `.vscode` folder of your project with the following default configuration:

    ```json
    {
        // Use IntelliSense to learn about possible attributes.
        // Hover to view descriptions of existing attributes.
        // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
        "version": "0.2.0",
        "configurations": [
            {
                "name": "Arm Debugger FVP",
                "type": "arm-debugger.fvp",
                "request": "launch",
                "cdbEntry": "<configuration database string of model to connect>",
                "program": "${command:arm-debugger.getApplicationFile}"
            }
        ]
    }
    ```

1. To open the **Debug Configuration** visual editor, click **Open Arm Debug Configuration** ![Debug Configuration icon](https://github.com/ARM-software/vscode-arm-debugger/raw/main/docs/images/run-debug-config-icon.png) in the top right-hand corner of the text editor.

    ![Debug Configuration editor](https://github.com/ARM-software/vscode-arm-debugger/raw/main/docs/images/debug-config-editor-fvp.png)

1. Use the visual editor to enter the following details:

    - **Target** section:

        - **Select Target**: Select the FVP that you want to use (for example, `MPS2_Cortex_M4`), then select a processor (for example, `Cortex-M4`). The list of FVPs available depends on the Arm Debugger configuration database installed and the `avh-fvp` version specified in the `vcpkg-configuration.json` file for your project. Use the **Show only installed** checkbox to filter the list of FVPs to display only the models installed on your machine.

    - **Application** section:

        - **Program Files**: Click **Add File** to point to an AXF or ELF file. You can add as many files as you need. The Arm Debugger extension uses the files in the order in which you added them. You can also use `${command:arm-debugger.getApplicationFile}`. This command detects the AXF or ELF file to use or offers a selection of files available if several AXF or ELF files are present in your workspace.

1. Save your changes.

    The options you select in the editor are added in the `launch.json` file.

For information on more options, see [Debug configuration options in the visual editor](https://developer.arm.com/documentation/108029/latest/Arm-Debugger-extension/Debug-your-project-with-Arm-Debugger/Modify-the-debug-configuration-options-with-the-Debug-Configuration-visual-editor/Debug-configuration-options-in-the-visual-editor) and [Arm Debugger debug configuration options](https://developer.arm.com/documentation/108029/latest/Arm-Debugger-extension/Debug-your-project-with-Arm-Debugger/Arm-Debugger-debug-configuration-options---CMSIS-use-cases).

**Note**: Alternatively, you can start from one of the official examples available on [Arm-Examples](https://github.com/Arm-Examples) or [keil.arm.com](https://www.keil.arm.com/boards/). The official examples contain pre-configured `launch.json` files.

## Start a debug session

### Start a debug session with a physical target

To start a debug session with a physical target:

1. Go to the **RUN AND DEBUG** view ![Run and Debug icon](https://github.com/ARM-software/vscode-arm-debugger/raw/main/docs/images/run-debug-icon.png) and select your configuration in the list ![Configuration](https://github.com/ARM-software/vscode-arm-debugger/raw/main/docs/images/start-debugging-button.png), then click **Start Debugging** ![Start Debugging](https://github.com/ARM-software/vscode-arm-debugger/raw/main/docs/images/start-debugging-button-02.png).

    The debug session starts. The debugger stops by default at the main() function of the program.

1. To see the debugger log output and use advanced debugger functionality with command-line interface commands, go to the **Debug Console** tab. See [Use the Debug Console](https://developer.arm.com/documentation/108029/latest/Arm-Debugger-extension/Debug-your-project-with-Arm-Debugger/Use-the-Debug-Console) for more details.

### Start a debug session with a virtual target

**Note:** FVPs are natively available on Windows and Linux only. If you are on a Mac, you can run FVPs in a Linux container with Docker. To install Docker and clone the [https://github.com/Arm-Examples/FVPs-on-Mac](https://github.com/Arm-Examples/FVPs-on-Mac) repository, follow this [Arm Developer Learning Path](https://learn.arm.com/install-guides/fvps-on-macos/).

To start a debug session with a virtual target:

1. If you are on a Mac, make sure Docker is running.

1. Go to the **RUN AND DEBUG** view ![Run and Debug icon](https://github.com/ARM-software/vscode-arm-debugger/raw/main/docs/images/run-debug-icon.png) and select your configuration in the list ![Configuration](https://github.com/ARM-software/vscode-arm-debugger/raw/main/docs/images/start-debugging-button-fvp.png), then click **Start Debugging** ![Start Debugging](https://github.com/ARM-software/vscode-arm-debugger/raw/main/docs/images/start-debugging-button-02.png).

    The debug session starts. The debugger stops by default at the main() function of the program.

1. To see the debugger log output and use advanced debugger functionality with command-line interface commands, go to the **Debug Console** tab. See [Use the Debug Console](https://developer.arm.com/documentation/108029/latest/Arm-Debugger-extension/Debug-your-project-with-Arm-Debugger/Use-the-Debug-Console) for more details.

## Pre-releases

The Arm Debugger extension introduces pre-releases from version 1.3.0. The extension follows the recommended [versioning](https://code.visualstudio.com/api/working-with-extensions/publishing-extension#prerelease-extensions) scheme `Major.Minor.Patch` with:

- Arm Debugger extension release: `Minor` is an even number, for example `1.2.0`.

- Arm Debugger extension pre-release: `Minor` is an odd number, for example `1.3.0`.

## Limitations

Some capabilities of the Arm Debugger engine that are not yet exposed in Visual Studio Code include:

- CoreSight™ trace

- Symmetric and asymmetric multicore debug

- RTOS awareness

## Submit feedback or report issues

To submit feedback or report issues on the Arm Debugger extension, please use [GitHub issues](https://github.com/Arm-Software/vscode-arm-debugger/issues) in the extension repository.
