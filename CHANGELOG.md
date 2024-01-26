# Change Log

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
