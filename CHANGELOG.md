# Change Log

## [Unreleased]

- Implements code lenses for the `launch.json` and `tasks.json` configuration files
- Improves the **Run Configuration** and **Debug Configuration** editors for UI and UX components
- Improves the run and debug connection flows:

    - Introduces a quick pick menu when creating a new configuration
    - Improves layout in the **Run Configuration** and **Debug Configuration** editors
    - Aligns configuration fields with Arm Debugger terminology:

        - `probeType` → `connectionType`
        - `serialNumber` → `connectionAddress`
        - `debugFrom` → `runControl`

    - Implements `runControl` options `debugFromEntrypoint`, `debugFromSymbol`, and `connectOnly` for the `launch.json` file and the **Debug Configuration** editor
    - Removes `cdbEntry` from the `arm-debugger` debug configuration
    - Implements field validation for the `launch.json` file and the **Debug Configuration** editor

- Enables the `arm-debugger.configdb` debug configuration for both model and hardware connections
- Removes the `arm-debugger.fvp` debug configuration for the `launch.json` file and the **Debug Configuration** editor
- Simplifies the attach configuration: Removes the `arm-debugger` attach debug configuration from the **Debug Configuration** editor
- Enables connections to any external Arm Debugger instance using the `connectExistingDebugger` and `debuggerAddress` fields in the `launch.json` file and the **Debug Configuration** editor
- Improves performance by allowing multiple debug connections using a single instance of Arm Debugger
- Removes the `Configuration Database: Disable Default` option from the extension settings

## 1.4.4

- Allows debug connections without setting `program` or `programs`. Ignores `debugFrom` in such cases.
- Renamed context menu entries `Open Debug Configuration` and `Open Run Configuration` to `Open Arm Debug Configuration` and `Open Arm Run Configuration`.
- Improves error messages.
- Shows notification when Arm Debugger process is starting up.
- Updates default Arm Debugger `launch` configuration for type `arm-debugger.fvp` to include a `cdbEntry` placeholder.

## 1.4.3

- Adds the possibility to manage additional Arm Debugger Configuration Databases with the **Configuration Database: Additional Locations** and **Configuration Database: Disable Default** extension settings.

## 1.4.2

- Improves the generation of FVP parameter files. Selecting an FVP in the **Device Manager** panel is no longer required.
- Improves the handling of empty `launch.json` and `tasks.json` files by the **Run Configuration** and **Debug Configuration** visual editors.
- Improves various display texts.

## 1.4.1

- Improves performance of the debug configuration GUI.

## 1.4.0

- Fixes issue when starting a debug session if log level was set to `off`, `error` or `warn`.
- Fixes issue with aborting start of a debug session if receiving error log messages from Arm Debugger.
- Adds support for Arm Debugger v6.3.0 release.
- Adds `Target Initialization Script` and `Debug Initialization Script` options to debug configuration GUI.
- Adds section to `README.md` to announce introduction of extension pre-releases.
- Replaces `Open Run and Debug Configuration` context menu entry with file-sensitive `Open Debug Configuration` and `Open Run Configuration` entries.
- Updates `Debugger Path` extension setting to locate the Arm Debugger Target Configuration Database and Jython imports.
- Updates `probe` debug and run configuration setting to default to `auto`. `auto` resolves to `CMSIS-DAP` if probe type cannot be automatically detected.
- Updates error handling for run and debug operations if Arm Debugger is neither in active tools environment, nor configured by `Debugger Path` extension setting.
- Updates extension setting descriptions.

## 1.2.2

- Allows setting breakpoints in assembler files (`.s`, `.S`).
- Enables different jython templates to be opened from the 'New File' menu.
- Adds Sort options to the function view.
- Adds support for vscode variables to `fvpParameters` setting in `launch.json`

## 1.2.1

- Fixes a regression in the debug configuration GUI for Fixed Virtual Platforms.
- Skips applying flash algorithm settings for Arm Debugger versions prior to v6.1.0.

## 1.2.0

- Adds link to the latest [Arm Debugger Release Notes](https://developer.arm.com/documentation/109667/latest) to the extension README.
- Adds initial version of the `Functions` window.
- Adds `eraseMode`, `verifyFlash`, and `flms` to the run configuration options and GUI to configure the use of [flash algorithms](https://open-cmsis-pack.github.io/Open-CMSIS-Pack-Spec/main/html/flashAlgorithm.html) from the selected CMSIS [Device Family Pack](https://open-cmsis-pack.github.io/Open-CMSIS-Pack-Spec/main/html/cp_PackTutorial.html#createPack_DFP).
- Adds Arm Debugger Jython debug script imports to `PYTHONPATH` environment variable of active session. This allows intellisense support for these debugger scripts in combination with for example the [Microsoft Python extension](https://marketplace.visualstudio.com/items?itemName=ms-python.python).
- Adds error message in debug configuration GUI for Fixed Virtual Platforms if no models are detected.
- Various fixes and improvements to the debug and run configuration GUI.

## 1.1.2

- Fixes a bug where Arm Debugger extension may not detect the path of the Arm Debugger executable provided by Arm Environment Manager.
- Fixes a sometimes empty `Configuration Database Entry` selection in the debug configuration GUI for debug type `arm-debugger.fvp`.
- Fixes focus to `Debug Console` when launching debug.

## 1.1.1

- Fixes previous version number in `CHANGELOG.md`.

## 1.1.0

- Debug and run configuration GUI enhancements and defect fixes.
- Improves error handling of debug and run configuration GUI.
- Changes debug and run configuration GUI to not automatically save changes. Use the Visual Studio Code [Auto Save](https://code.visualstudio.com/docs/editor/codebasics#_save-auto-save) feature instead.
- Includes `Arm Debugger` in command and task names to distinguish from other debug extensions.
- Adds installation of required CMSIS [Device Family Packs](https://open-cmsis-pack.github.io/Open-CMSIS-Pack-Spec/main/html/cp_PackTutorial.html#createPack_DFP) for debug and run for standalone use of this extension.
- Includes `arm.environment-manager` extension in this extension pack instead of being a dependency.
- The following changes require Arm Debugger v6.1.1 or later:
    - Adds `targetInitScript` to debug and to run configuration options.
    - Adds `debugInitScript` to debug configuration options.
    - Adds [`dbgconf`](https://open-cmsis-pack.github.io/Open-CMSIS-Pack-Spec/main/html/dbg_debug_sqns.html#dbg_sqns_dbgconf) to to debug and to run configuration options for CMSIS-Pack based hardware connections.

## 1.0.0

- Removes preview status of the extension.
- Adds new debug type `arm-debugger.fvp` to enable debug of Fixed Virtual Platforms. Works best in combination with `arm.virtual-hardware` extension.
- Adds support for new debug type `arm-debugger.fvp` to debug configuration GUI.
- Adds editing capabilities to `Registers` window.
- Adds various enhancements and fixes to the debug and run configuration GUI.
- Adds support for more configuration options to the debug and run configuration GUI.
- Updates default Arm Debugger `launch` configuration to use new commands from related extensions.
- Updates `programs` debug and run configuration option to accept a string with a comma-separated list of file paths.
- Fixes run operation for applications with spaces in their path ([GH Issue #1](https://github.com/ARM-software/vscode-arm-debugger/issues/1)).

## 0.5.1

- Fixes usage of `probe` detection for debug with `cdbEntry`.

## 0.5.0

- Adds initial version of debug and run configuration GUI. Available through context menu for `launch.json` and `tasks.json` files.
- Detects probe type based on given `serialNumber` if no `probe` option set in debug or in run configuration. Defaults to `CMSIS-DAP` if type not known.
- Adds `debugPortMode` to debug and to run configuration options to switch between SWD and JTAG.
- Adds `debugClockSpeed` to debug and to run configuration options to limit the debug clock to a maximum frequency.
- Adds `resetAfterConnect` to debug configuration options to decide if to reset the device after connection and before running to `debugFrom`.
- Adds `cdbEntryParams` to debug configuration options.
- Adds `workspaceFolder` to run configuration options.
- Create single Arm Debugger `launch` configuration if no `launch.json` file present.
- Deprecates `cmsisDevice` debug option. Switch to use of already existing `cmsisPack`, `pdsc`, `vendorName`, `deviceName`, `processorName` options instead.

## 0.4.0

- Adds query of `deviceName` and `processorName` debug configuration options from Arm CMSIS csolution extension if not provided by user.
- Adds support for flash download of multiple ELF files via `programs` run configuration option. Requires Arm Debugger for Visual Studio Code v6.0.2 or later.
- Improves display of Registers view values.
- Shows Output channel only when it has content.
- Fixes flash download with ULINKpro and ULINKpro D.
- Fixes potential Registers view lockup after run or step operation.
- Fixes Arm Debugger shutdown when closing Visual Studio Code.

## 0.3.0

- Support for Eclipse CDT-Cloud Peripheral Inspector.
- Display CPU registers in separate Registers view.

## 0.2.0

- Debug type changed to `arm-debugger`
- `arm.environment-manager` extension added as dependency.
- Detection if Arm Debugger is installed on the system added.
- Gracefully stop debugger if Arm Debugger is not installed.
- Automatic install of Arm Debugger dialog added.

## 0.1.0

- Initial pre-release.
