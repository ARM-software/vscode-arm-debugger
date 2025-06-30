# Arm Debugger

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Install the extension](#install-the-extension)
- [Install the Arm Debugger engine and other tools with vcpkg](#install-the-arm-debugger-engine-and-other-tools-with-vcpkg)
- [Define a run configuration and run a project on your board](#define-a-run-configuration-and-run-a-project-on-your-board)
- [Define and start a debug connection](#define-and-start-a-debug-connection)
    - [Work with a physical target](#work-with-a-physical-target)
        - [Built-in platform option](#built-in-platform-option)
        - [CMSIS-Pack device option](#cmsis-pack-device-option)
    - [Work with a virtual target](#work-with-a-virtual-target)
    - [Attach to an existing connection](#attach-to-an-existing-connection)
- [Limitations](#limitations)
- [Submit feedback or report issues](#submit-feedback-or-report-issues)

## Quality

| Badge           | Status                                                                                                                                                           |
| --------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Maintainability | [![Maintainability](https://qlty.sh/badges/be5d1912-6185-4375-bcbf-fa0804ea034b/maintainability.png)](https://qlty.sh/gh/Arm-Debug/projects/vscode-arm-debugger) |
| Code Coverage   | [![Code Coverage](https://qlty.sh/badges/be5d1912-6185-4375-bcbf-fa0804ea034b/test_coverage.png)](https://qlty.sh/gh/Arm-Debug/projects/vscode-arm-debugger)     |

## Overview

The Arm® Debugger extension allows you to debug and optimize software running on Arm-based processors. The extension can be used independently and is also compatible with other extensions included in the Keil Studio Pack.

The extension provides access to the Arm Debugger engine by implementing the [Microsoft Debug Adapter Protocol (DAP)](https://microsoft.github.io/debug-adapter-protocol//).

The Arm Debugger engine supports connections to the following targets:

- Physical targets: Either through external debugging, for example, Arm ULINK™ or DSTREAM debug probes, or through on-board low-cost debugging, for example, ST-Link or CMSIS-DAP based debug probes.

- Virtual targets: Using Fixed Virtual Platform models (FVPs).

See the [Arm Debugger Release Note](https://developer.arm.com/documentation/109667/latest) for an overview of each Arm Debugger engine release.

### Supported features

The Arm Debugger extension allows you to:

- Load images like .elf, .axf, or .bin files and debug information according to the DWARF Debugging Information Format Standard, up to and including version 5

- Run images

- Set breakpoints

- Do source and instruction stepping

- Access variables and registers

- View the contents of memory

- Debug virtual targets (FVPs)

- Access the [CLI](https://developer.arm.com/documentation/101471/latest/Arm-Debugger-commands) using the Debug Console

### Supported debug connections

The Arm Debugger engine supports debug connections based on the following information:

- The [debug setup included in the CMSIS-Packs](https://open-cmsis-pack.github.io/Open-CMSIS-Pack-Spec/main/html/coresight_setup.html) of your project

- The integrated configuration database (configdb). The configuration database is where Arm Debugger stores information about the processors, devices, and boards that it can connect to.

### Supported technologies

For a complete list of supported architectures, processors, Fixed Virtual Platform models (FVPs), and debug probes see the [Arm Debugger Release Note](https://developer.arm.com/documentation/109667/latest).

## Prerequisites

You must install the Arm Debugger extension (identifier: `arm.arm-debugger`).

The following extensions are installed automatically alongside Arm Debugger:

- Arm Tools Environment Manager (identifier: `arm.environment-manager`)

- Arm Device Manager (identifier: `arm.device-manager`)

- Arm CMSIS Solution (identifier: `arm.cmsis-csolution`)

- Peripheral Inspector (identifier: `eclipse-cdt.peripheral-inspector`)

- Memory Inspector (identifier: `eclipse-cdt.memory-inspector`)

To debug a virtual target using FVPs, you must have the models installed locally. See [Work with a virtual target](#work-with-a-virtual-target) for more details.

You must have an example project ready to use. You can create an example from scratch. Alternatively, if you are working with CMSIS, you can start from one of the official examples available on [Arm Examples](https://github.com/Arm-Examples) or [keil.arm.com](https://www.keil.arm.com/boards/). These examples contain pre-configured `vcpkg-configuration.json`, `tasks.json`, and `launch.json` files.

## Install the extension

1. Open Visual Studio Code Desktop and click the **Extensions** icon ![Extensions icon](https://github.com/ARM-software/vscode-arm-debugger/raw/main/docs/images/extensions-icon.png) in the Activity Bar to open the **Extensions** view.

1. Search for **Arm Debugger** and click **Install**.

    Arm Debugger and all the extensions contained in the Arm Debugger extension pack are installed at the same time.

## Install the Arm Debugger engine and other tools with vcpkg

The Arm Tools Environment Manager extension downloads, installs, and manages software development tools, including the Arm Debugger engine, using [Microsoft vcpkg](https://vcpkg.io/en/index.html) artifacts. Arm Tools Environment Manager adds a `vcpkg-configuration.json` manifest file to your project and uses this file to acquire and activate the tools needed to set up your development environment.

Some tools, like the Arm Debugger engine, require a [user-based licensing (UBL)](https://developer.arm.com//Tools%20and%20Software/User-based%20Licensing) license. You must activate the license with the **Arm License Management Utility** available with Arm Tools Environment Manager.

See the [Arm Tools Environment Manager README](https://marketplace.visualstudio.com/items?itemName=Arm.environment-manager) for more details.

To install the Arm Debugger engine and other tools:

1. Open the project that you want to work on.

1. Click the **Arm Tools** status bar item and select **Add Arm Tools Configuration to Workspace** in the drop-down list at the top of the window.

    A visual editor opens where you can select the tools you need.

1. Select the latest version available for **Arm Debugger** and select the other tools you require.

    A `vcpkg-configuration.json` file is added to your project and the tools that you selected are included in the file. You can also update the file manually. Click the `vcpkg-configuration.json` tab to display the file contents.

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
            "arm:debuggers/arm/armdbg": "6.5.0"
        }
    }
    ```

1. Save your changes.

    The Arm Tools Environment Manager extension activates the workspace and downloads the tools specified in the `vcpkg-configuration.json` file. After installation, tools are available on the PATH.

1. The status bar shows **No Arm License** if no valid UBL license is active. In this case, click **No Arm License** and select **Activate or manage Arm licenses** in the drop-down list at the top of the window.

1. Manage your license with the **Arm License Management Utility**.

    ![Status bar](https://github.com/ARM-software/vscode-arm-debugger/raw/main/docs/images/status-bar.png)

## Define a run configuration and run a project on your board

To download your code and run it on your hardware, you must first configure a task. Visual Studio Code uses a `tasks.json` configuration file to run projects. Use the **Run Configuration** visual editor to select options. Alternatively, you can add the configuration manually in the `tasks.json` file.

**Note**: In this procedure, it is assumed that you have already built either a PDSC file or an AXF or ELF file using your toolchain.

To define a run configuration and run a project on your board:

1. Open the Command Palette. Search for `Tasks: Configure Task` and then select it.

1. Select the `arm-debugger.flash: Flash Device` task (or **Flash Device**).

    Visual Studio Code creates a `tasks.json` file in the `.vscode` folder of your project with the following default configuration:

    ```json
    {
        "version": "2.0.0",
        "tasks": [
            {
                "type": "arm-debugger.flash",
                "connectionAddress": "${command:device-manager.getSerialNumber}",
                "program": "${command:arm-debugger.getApplicationFile}",
                "cmsisPack": "${command:cmsis-csolution.getTargetPack}",
                "deviceName": "${command:cmsis-csolution.getDeviceName}",
                "processorName": "${command:cmsis-csolution.getProcessorName}",
                "problemMatcher": [],
                "label": "arm-debugger.flash: Flash Device"
            }
        ]
    }
    ```

1. To open the **Run Configuration** visual editor, either:

    - Click the **Open in editor** code lens above the configuration. Code lenses are enabled by default with the **Enable Code Lens** Arm Debugger setting.

    - Click **Open Arm Run Configuration** ![Run Configuration icon](https://github.com/ARM-software/vscode-arm-debugger/raw/main/docs/images/run-debug-config-icon.png) in the top right-hand corner of the text editor.

    ![Run Configuration editor](https://github.com/ARM-software/vscode-arm-debugger/raw/main/docs/images/run-config-editor.png)

1. Use the visual editor to enter the following details:

    - **CMSIS-Pack/Device** section:

        - **CMSIS-Pack**: Select the Device Family Pack (DFP) for the target debug probe or board.

          - Default value: `auto (CMSIS Solution)`. The Arm Debugger extension uses the DFP for the active device defined in the `*.csolution.yml` file of your solution. The Arm Debugger extension adds the `"${command:cmsis-csolution.getTargetPack}"` command in the JSON file for `"cmsisPack"`.

          - `auto (Device Manager)`: The Arm Debugger extension uses the DFP for the active device in the Arm Device Manager extension. The Arm Debugger extension adds the `"${command:device-manager.getDevicePack}"` command in the JSON file for `"cmsisPack"`.

          - You can also select the DFP for the active device in the drop-down list. Alternatively, type the name of the DFP in the format `<vendor>::<pack>@<version>`. For example: `ARM::Cortex_DFP@1.1.0`.

        - **CMSIS-Pack Device Name**: Select the name of the target device, that is, the target chip on your board.

          - Default value: `auto (CMSIS Solution)`. The Arm Debugger extension detects the device name from the information available for the probe or board in the `*.csolution.yml` file of your solution. The Arm Debugger extension adds the `"${command:cmsis-csolution.getDeviceName}"` command in the JSON file for `"deviceName"`.

          - `auto (Device Manager)`: The Arm Debugger extension detects the device name from the information available for the probe or board in the Arm Device Manager extension. The Arm Debugger extension adds the `"${command:device-manager.getDeviceName}"` command in the JSON file for `"deviceName"`.

          - You can also select the device name in the drop-down list. Alternatively, type the device name. For example: `MPS3_SSE_300`. The device name available in the drop-down list is the one defined in the `*.csolution.yml` file of your solution.

        - **Processor Name**: For multicore devices, type the name of the processor to use.

          - Default value: `auto (CMSIS Solution)`. The Arm Debugger extension uses the processor name that is defined in the `*.csolution.yml` file of your solution. The Arm Debugger extension adds the `"${command:cmsis-csolution.getProcessorName}"` command in the JSON file for `"processorName"`.

          - You can also select the processor name in the drop-down list. Alternatively, type the processor name. For example: `cm4`. The processor names available in the drop-down list are the ones defined in the `*.csolution.yml` file of your solution.

    - **Connections** section:

        - **Connection Address**: Select the serial number of the connected debug probe or debug unit.

          - Default value: `auto`. With `auto`, the Arm Debugger extension uses the serial number of the active device in the Arm Device Manager extension by default. The Arm Debugger extension adds the `"${command:device-manager.getSerialNumber}"` command in the JSON file for `"connectionAddress"`.

          - You can also select the serial number of the active device in the drop-down list. Alternatively, type a connection address or serial number manually.

    - **Application** section:

        - **Program Files**: Click **Add File** to point to an AXF or ELF file. You can add as many files as you need. The Arm Debugger extension uses the files in the order in which you added them. You can also click **Detect** to use `${command:arm-debugger.getApplicationFile}`. This command detects the AXF or ELF file to use or offers a selection of files available if several AXF or ELF files are present in your workspace.

1. Save your changes.

    The options that you select in the editor are added in the `tasks.json` file.<!-- Links to standalone guide to add: For information on more options, see "Run configuration options in the visual editor" and "Arm Debugger run configuration options".-->

1. To run the project on your board:

    - Open the Command Palette. Search for `Tasks: Run Task` and then select it.

    - Select the `arm-debugger.flash: Flash Device` task (or **Flash Device**).

    **Note**: When you run a task for the first time, Visual Studio Code asks you to select a scanning option for the task output. Select the `Continue without scanning the task output` option in the drop-down list.

    - If you are using a multicore device and you did not specify a `"processorName"` in the `tasks.json` file, select the appropriate processor for your device in the **Select a processor** drop-down list at the top of the window.

    The project is downloaded to the board and starts running.

1. Check the download progress in the **Terminal** tab.

## Define and start a debug connection

Visual Studio Code uses a `launch.json` configuration file to define debugger settings. You can use the **Debug Configuration** visual editor to choose configuration options, or manually edit the `launch.json` file.

### Arm Debugger Launch

With the **Arm Debugger Launch** option, you can start your program in debug mode using various configurations that support both physical and virtual targets.

#### Physical targets

Select **Hardware debug connection**, then select:

- **Built-in platform**: Connect to an off-the-shelf development board or a platform defined in the Arm Debugger configuration database. This option provides a pre-defined, integrated debug setup, and adds an `"arm-debugger.configdb"` configuration with a `"launch"` request to the `launch.json` file.

- **CMSIS-Pack device**: Connect using a CMSIS-Pack, specifically a Device Family Pack (DFP) provided by silicon vendors. This method requires a `*.csolution.yml` file specifying the target device, compiler, and debug settings. It adds an `"arm-debugger"` configuration with a `"launch"` request to the `launch.json` file.

#### Virtual targets

Select **Model debug connection** to use Fixed Virtual Platform models (FVPs) to debug virtual targets. This adds an `"arm-debugger.configdb"` configuration with a `"launch"` request to the `launch.json` file.

### Arm Debugger Attach

With the **Arm Debugger Attach** option, you can attach to a debug connection that is already established. This adds an `"arm-debugger"` configuration with an `"attach"` request to the `launch.json` file. Unlike launch configurations, you must configure this type manually and cannot use the visual editor.

### Work with a physical target

#### Built-in platform option

To define and start a debug connection:

1. Open the Command Palette. Search for `Debug: Add Configuration...` and then select it.

1. Select `Arm Debugger` in the drop-down list that displays.

1. Select **Arm Debugger Launch** > **Hardware debug connection** > **Built-in platform**.

1. Validate the name of the configuration and press **Enter**.

    Visual Studio Code creates a `launch.json` file in the `.vscode` folder of your project with the following default configuration:

    ```json
    {
        // Use IntelliSense to learn about possible attributes.
        // Hover to view descriptions of existing attributes.
        // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
        "version": "0.2.0",
        "configurations": [
            {
                "name": "Arm Debugger Built-in Platform",
                "type": "arm-debugger.configdb",
                "request": "launch",
                "cdbEntry": "<Select platform>",
                "connectionAddress": "<Enter address>"
            }
        ]
    }
    ```

1. To open the **Debug Configuration** visual editor, either:

    - Click the **Open in editor** code lens above the configuration. Code lenses are enabled by default with the **Enable Code Lens** Arm Debugger setting.

    - Click **Open Arm Debug Configuration** ![Debug Configuration icon](https://github.com/ARM-software/vscode-arm-debugger/raw/main/docs/images/run-debug-config-icon.png) in the top right-hand corner of the text editor.

    ![Debug Configuration editor](https://github.com/ARM-software/vscode-arm-debugger/raw/main/docs/images/debug-config-editor-configdb.png)

1. Use the visual editor to enter the following details:

    - **Connections** section:

        - **Select Target**: The configuration database is where Arm Debugger stores information about the processors, boards, and cores that it can connect to. The database is arranged by vendor. Drill down to select the core that you want to connect to.

        - **Bare Metal Debug** > **Connection Type**: Select a type for the debug probe that you are using or the debug unit on your board.

          - Default value: `DSTREAM Family` if DSTREAM is supported.

          - You can connect a probe or a board to your computer over USB. In this case, the Arm Debugger extension sets a probe type based on the configuration database entry selected in the **Select Target** drop-down list.

        - **Bare Metal Debug** > **Debug Port Mode**: Select a debug port mode to use. A debug port allows you to communicate with and debug microcontrollers or other embedded systems.

          - Default value: `JTAG` or `SWD` (read only). The option that displays depends on the board that you have selected.

          - `JTAG`: Use the JTAG debug port mode.

          - `SWD`: Use the SWD debug port mode.

        - **Bare Metal Debug** > **Clock Speed**: The maximum clock frequency for the debug communication. The clock frequency is the speed at which data is transferred between the debugger and the target device during debugging operations. The frequency actually used depends on the capabilities of the debug probe and might be reduced to the next supported frequency.

          - Default value: `auto`. With `auto`, the debugger decides which clock frequency to use based on the connected target device.

          - Other possible values: `50MHz`, `33MHz`, `25MHz`, `20MHz`, `10MHz`, `5MHz`, `2MHz`, `1MHz`, `500kHz`, `200kHz`, `100kHz`, `50kHz`, `20kHz`, `10kHz`, `5kHz`.

        - **Bare Metal Debug** > **Connection Address**: Select the serial number of the connected debug probe or debug unit.

          - Select `auto` in the drop-down list. With `auto`, the Arm Debugger extension uses the serial number of the active device in the Arm Device Manager extension by default. The Arm Debugger extension adds the `"${command:device-manager.getSerialNumber}"` command in the JSON file for `"connectionAddress"`.

          - You can also enter the serial number or connection address of the active device.

    - **Application** section:

        - **Program Files**: Click **Add File** to point to an AXF or ELF file. You can add as many files as you need. The Arm Debugger extension uses the files in the order in which you added them. You can also click **Detect** to use `${command:arm-debugger.getApplicationFile}`. This command detects the AXF or ELF file to use or offers a selection of files available if several AXF or ELF files are present in your workspace.

    - **Debugger** section:

        - **Run Control**: As you have selected an AXF or ELF file in **Program Files**, select **Debug From Entrypoint** or **Debug From Symbol**. The debugger can either start from the entry point of the program or run until it reaches a specified symbol (defaults to main) before pausing. 

        **Note**: The **Connect Only** option is set by default and does not require you to select a file in **Program Files**.

1. Save your changes.

    The options that you select in the editor are added in the `launch.json` file.<!-- Links to standalone guide to add: For information on other options, see the **Launch ConfigDB configuration for physical targets** options in "Debug configuration options in the visual editor" and "Arm Debugger ConfigDB debug configuration options".-->

1. To start a debug session, click **Start debugging** in the top right-hand corner of the **Debug Configuration** visual editor.

    The debug session starts. The debugger stops at the location that you specified in **Run Control**.

1. To see the debugger log output and use advanced debugger functionality with command-line interface commands, check the **Debug Console** tab. <!-- Link to standalone guide to add: See "Use the Debug Console" for more details.-->

#### CMSIS-Pack device option

To define and start a debug connection:

1. Open the Command Palette. Search for `Debug: Add Configuration...` and then select it.

1. Select `Arm Debugger` in the drop-down list that displays.

1. Select **Arm Debugger Launch** > **Hardware debug connection** > **CMSIS-Pack device**.

1. Validate the name of the configuration and press **Enter**.

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
                "connectionAddress": "${command:device-manager.getSerialNumber}",
                "program": "${command:arm-debugger.getApplicationFile}",
                "cmsisPack": "${command:cmsis-csolution.getTargetPack}",
                "deviceName": "${command:cmsis-csolution.getDeviceName}",
                "processorName": "${command:cmsis-csolution.getProcessorName}"
            }
        ]
    }
    ```

1. To open the **Debug Configuration** visual editor, either:

    - Click the **Open in editor** code lens above the configuration. Code lenses are enabled by default with the **Enable Code Lens** Arm Debugger setting.

    - Click **Open Arm Debug Configuration** ![Debug Configuration icon](https://github.com/ARM-software/vscode-arm-debugger/raw/main/docs/images/run-debug-config-icon.png) in the top right-hand corner of the text editor.

    ![Debug Configuration editor](https://github.com/ARM-software/vscode-arm-debugger/raw/main/docs/images/debug-config-editor-cmsis.png)

1. Use the visual editor to enter the following details:

    - **CMSIS-Pack/Device** section:

        - **CMSIS-Pack**: Select the Device Family Pack (DFP) for the target debug probe or board.

          - Default value: `auto (CMSIS Solution)`. The Arm Debugger extension uses the DFP for the active device defined in the `*.csolution.yml` file of your solution. The Arm Debugger extension adds the `"${command:cmsis-csolution.getTargetPack}"` command in the JSON file for `"cmsisPack"`.

          - `auto (Device Manager)`: The Arm Debugger extension uses the DFP for the active device in the Arm Device Manager extension. The Arm Debugger extension adds the `"${command:device-manager.getDevicePack}"` command in the JSON file for `"cmsisPack"`.

          - You can also select the DFP for the active device in the drop-down list. Alternatively, type the name of the DFP in the format `<vendor>::<pack>@<version>`. For example: `ARM::Cortex_DFP@1.1.0`.

        - **CMSIS-Pack Device Name**: Select the name of the target device, that is, the target chip on your board.

          - Default value: `auto (CMSIS Solution)`. The Arm Debugger extension detects the device name from the information available for the probe or board in the `*.csolution.yml` file of your solution. The Arm Debugger extension adds the `"${command:cmsis-csolution.getDeviceName}"` command in the JSON file for `"deviceName"`.

          - `auto (Device Manager)`: The Arm Debugger extension detects the device name from the information available for the probe or board in the Arm Device Manager extension. The Arm Debugger extension adds the `"${command:device-manager.getDeviceName}"` command in the JSON file for `"deviceName"`.

          - You can also select the device name in the drop-down list. Alternatively, type the device name. For example: `MPS3_SSE_300`. The device name available in the drop-down list is the one defined in the `*.csolution.yml` file of your solution.

        - **Processor Name**: For multicore devices, type the name of the processor to use.

          - Default value: `auto (CMSIS Solution)`. The Arm Debugger extension uses the processor name that is defined in the `*.csolution.yml` file of your solution. The Arm Debugger extension adds the `"${command:cmsis-csolution.getProcessorName}"` command in the JSON file for `"processorName"`.

          - You can also select the processor name in the drop-down list. Alternatively, type the processor name. For example: `cm4`. The processor names available in the drop-down list are the ones defined in the `*.csolution.yml` file of your solution.

    - **Connections** section:

        - **Connection Type**: Select a type for the debug probe that you are using or the debug unit on your board.

          - Default value: `auto`. If the Arm Debugger extension cannot set the probe type automatically, the default value is `CMSIS-DAP`.

          - You can connect a probe or a board to your computer over USB. In this case, the Arm Debugger extension sets a probe type based on the serial number of the hardware detected.

        - **Debug Port Mode**: Select a debug port mode to use. A debug port allows you to communicate with and debug microcontrollers or other embedded systems.

          - Default value: `auto`. With `auto`, the debugger decides which debug port mode to use based on the connected target device.

          - `JTAG`: Use the JTAG debug port mode.

          - `SWD`: Use the SWD debug port mode.

        - **Clock Speed**: The maximum clock frequency for the debug communication. The clock frequency is the speed at which data is transferred between the debugger and the target device during debugging operations. The frequency actually used depends on the capabilities of the debug probe and might be reduced to the next supported frequency.

          - Default value: `auto`. With `auto`, the debugger decides which clock frequency to use based on the connected target device.

          - Other possible values: `50MHz`, `33MHz`, `25MHz`, `20MHz`, `10MHz`, `5MHz`, `2MHz`, `1MHz`, `500kHz`, `200kHz`, `100kHz`, `50kHz`, `20kHz`, `10kHz`, `5kHz`.

        - **Connection Address**: Select the serial number of the connected debug probe or debug unit.

          - Default value: `auto`. With `auto`, the Arm Debugger extension uses the serial number of the active device in the Arm Device Manager extension by default. The Arm Debugger extension adds the `"${command:device-manager.getSerialNumber}"` command in the JSON file for `"connectionAddress"`.

          - You can also select the serial number of the active device in the drop-down list. Alternatively, type a connection address or serial number manually.

    - **Application** section:

        - **Program Files**: Click **Add File** to point to an AXF or ELF file. You can add as many files as you need. The Arm Debugger extension uses the files in the order in which you added them. You can also click **Detect** to use `${command:arm-debugger.getApplicationFile}`. This command detects the AXF or ELF file to use or offers a selection of files available if several AXF or ELF files are present in your workspace.

    - **Debugger** section:

        - **Run Control**: As you have selected an AXF or ELF file in **Program Files**, select **Debug From Entrypoint** or **Debug From Symbol**. The debugger can either start from the entry point of the program or run until it reaches a specified symbol (defaults to main) before pausing.

        **Note**: The **Connect Only** option does not require you to select a file in **Program Files**.

1. Save your changes.

    The options that you select in the editor are added in the `launch.json` file.<!-- Links to standalone guide to add: For information on other options, see the **Launch configuration** options in "Debug configuration options in the visual editor" and "Arm Debugger debug configuration options".-->

1. To start a debug session, click **Start debugging** in the top right-hand corner of the **Debug Configuration** visual editor.

    The debug session starts. The debugger stops at the location that you specified in **Run Control**.

1. To see the debugger log output and use advanced debugger functionality with command-line interface commands, check the **Debug Console** tab. <!-- Link to standalone guide to add: See "Use the Debug Console" for more details.-->

### Work with a virtual target

**Note**: FVPs are natively available on Windows and Linux only. If you are on a Mac, you can run FVPs in a Linux container with Docker. To install Docker and clone the [https://github.com/Arm-Examples/FVPs-on-Mac](https://github.com/Arm-Examples/FVPs-on-Mac) repository, follow this [Arm Developer Learning Path](https://learn.arm.com/install-guides/fvps-on-macos/).

To define and start a debug connection:

1. To debug a virtual target using FVPs, you must have the models installed locally. The list of available FVPs depends on the version of Arm Debugger specified in the `vcpkg-configuration.json` file for your project. Arm Debugger includes a configuration database that provides access to these FVPs.

    **Note**: For Cortex&reg;-M based on Fast Models, add `"arm:models/arm/avh-fvp": "<version>"` in the `"requires":` section of the `vcpkg-configuration.json` file for your project.

1. Open the Command Palette. Search for `Debug: Add Configuration...` and then select it.

1. Select `Arm Debugger` in the drop-down list that displays.

1. Select **Arm Debugger Launch** > **Model debug connection**.

1. Validate the name of the configuration and press **Enter**.

    Visual Studio Code creates a `launch.json` file in the `.vscode` folder of your project with the following default configuration:

    ```json
    {
        // Use IntelliSense to learn about possible attributes.
        // Hover to view descriptions of existing attributes.
        // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
        "version": "0.2.0",
        "configurations": [
            {
                "name": "Connect to Model",
                "type": "arm-debugger.configdb",
                "request": "launch",
                "cdbEntry": "<configuration database string of model to connect>"
            }
        ]
    }
    ```

1. To open the **Debug Configuration** visual editor, either:

    - Click the **Open in editor** code lens above the configuration. Code lenses are enabled by default with the **Enable Code Lens** Arm Debugger setting.

    - Click **Open Arm Debug Configuration** ![Debug Configuration icon](https://github.com/ARM-software/vscode-arm-debugger/raw/main/docs/images/run-debug-config-icon.png) in the top right-hand corner of the text editor.

    ![Debug Configuration editor](https://github.com/ARM-software/vscode-arm-debugger/raw/main/docs/images/debug-config-editor-configdb-fvp.png)

1. Use the visual editor to enter the following details:

    - **Connections** section:

        - **Select Target**: Select the FVP that you want to use (for example, `MPS2_Cortex_M4`), then select a processor (for example, `Cortex-M4`). The list of available FVPs depends on the version of Arm Debugger specified in the `vcpkg-configuration.json` file.

    - **Application** section:

        - **Program Files**: Click **Add File** to point to an AXF or ELF file. You can add as many files as you need. The Arm Debugger extension uses the files in the order in which you added them. You can also click **Detect** to use `${command:arm-debugger.getApplicationFile}`. This command detects the AXF or ELF file to use or offers a selection of files available if several AXF or ELF files are present in your workspace.

    - **Debugger** section:

        - **Run Control**: As you have selected an AXF or ELF file in **Program Files**, select **Debug From Entrypoint** or **Debug From Symbol**. The debugger can either start from the entry point of the program or run until it reaches a specified symbol (defaults to main) before pausing.

       **Note**: The **Connect Only** option is set by default and does not require you to select a file in **Program Files**.

1. Save your changes.

    The options you select in the editor are added in the `launch.json` file.<!-- Links to standalone guide to add: For information on more options, see the **Launch ConfigDB configuration for FVPs** options in "Debug configuration options in the visual editor" and "Arm Debugger ConfigDB debug configuration options".-->

1. To start a debug session:

    - If you are on a Mac, make sure Docker is running.

    - Click **Start debugging** in the top right-hand corner of the **Debug Configuration** visual editor.

    The debug session starts. The debugger stops at the location that you specified in **Run Control**.

1. To see the debugger log output and use advanced debugger functionality with command-line interface commands, check the **Debug Console** tab. <!-- Link to standalone guide to add: See "Use the Debug Console" for more details.-->

### Attach to an existing connection

To attach to an existing connection:

1. Open the Command Palette. Search for `Debug: Add Configuration...` and then select it.

1. Select `Arm Debugger` in the drop-down list that displays.

1. Select **Arm Debugger Attach**.

1. Validate the name of the configuration and press **Enter**.

    Visual Studio Code creates a `launch.json` file in the `.vscode` folder of your project with the following default configuration:

    ```json
    {
        // Use IntelliSense to learn about possible attributes.
        // Hover to view descriptions of existing attributes.
        // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
        "version": "0.2.0",
        "configurations": [
            {
                "name": "Arm Debugger (Attach)",
                "type": "arm-debugger",
                "request": "attach",
                "address": "Websocket (ws://<host>:<port>) or socket (<host>:<port>) to connect to"
            }
        ]
    }
    ```

1. In the `launch.json` file, specify the address of the Arm Debugger process that you want to connect to. Use `ws://<host>:<port>` for a WebSocket connection or `<host>:<port>` for a plain socket connection.

1. Save your changes.

1. To attach to the debug connection:

    - Make sure that an Arm Debugger session is running.

    - Click the **Start debugging** code lens above the configuration in the `launch.json` file.

    The extension attaches to the running debug connection.

## Limitations

Some capabilities of the Arm Debugger engine that are not yet exposed in Visual Studio Code include:

- CoreSight™ trace

- Symmetric and asymmetric multicore debug

- RTOS awareness

## Submit feedback or report issues

To submit feedback or report issues on the Arm Debugger extension, please use [GitHub issues](https://github.com/Arm-Software/vscode-arm-debugger/issues) in the extension repository.
