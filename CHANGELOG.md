# Change Log

## NEXT RELEASE
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
