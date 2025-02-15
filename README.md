# React Native V8 JSI adapter
A V8 adapter implemention of the JSI interface for the react-native framework.

[![Build Status](https://dev.azure.com/ms/v8-jsi/_apis/build/status/microsoft.v8-jsi?branchName=master)](https://dev.azure.com/ms/v8-jsi/_build/latest?definitionId=321&branchName=master)

## Building

#### Windows
Run `./localbuild.ps1` in a PowerShell terminal; by default, this will only build the win32 x64 debug version of the binary. Edit the file to specify other platforms, architectures or flavors.

#### Android
From the `android` directory, run `./localbuild.sh` in a bash terminal; by default, this will build the x64 debug version of the binary for Android. To build for other platforms and flavors, supply the `--platform` (or `-p`) and the `--flavor` (or `-f`) flags, like so:

`./localbuild.sh --platform <platform-name> --flavor <flavor-name>`

The following platforms and flavors are supported:
*   `<platform-name>`: x64 (default), x86, arm, arm64
*   `<flavor-name>` debug (default), ship

This build requires **Ubuntu 18.04** or below, or **Debian 8** or later.

Currently, the Android version builds an older release of V8 (7.0.276.32) and uses Android NDK r21b and the JSI headers from React Native 0.66 (see [build.config](android/build.config)). The Windows version builds V8 9.5 and uses the JSI headers from React Native 0.65.5. Android also currently uses a different V8Runtime ([V8Runtime.h](android/V8Runtime.h)) than Windows ([V8JsiRuntime.h](src/public/V8JsiRuntime.h)). Future Android releases will sync the Android dependencies with Windows and support newer Linux versions.

##### Building with WSL2 on Windows 10/11
* [Enable](https://docs.microsoft.com/en-us/windows/wsl/install) Windows Subsystem for Linux
* Install debian: `wsl --install -d Debian`
* Install minimal dependencies on the Debian VM: `sudo apt install pkg-config python python3 curl unzip`
* Build with `./localbuild.sh` (as above)

##### [EXPERIMENTAL!] Building the main code with WSL2 on Windows 10/11
* [Enable](https://docs.microsoft.com/en-us/windows/wsl/install) Windows Subsystem for Linux
* Install debian: `wsl --install -d Debian`
* [Install PowerShell](https://docs.microsoft.com/en-us/powershell/scripting/install/install-debian?view=powershell-7.2) in the Debian VM
* Install minimal dependencies on the Debian VM: `sudo apt install lsb-release`
* Make sure you have at least 15Gb of disk space on the drive where the WSL image lives (usually C:)
* Build with `pwsh ./localbuild.ps1 -AppPlatform android`
* If setup is completed succesfully, build incrementally with `pwsh ./localbuild.ps1 -AppPlatform android -NoSetup`

### Out-of-sync issues
Until the JSI headers find a more suitable home, they're currently duplicated between the various repos. Code in jsi\jsi should be synchronized with the matching version of JSI from react-native (from https://github.com/facebook/hermes/tree/master/API/jsi/jsi).

### Build script patches
To regenerate after manual fix-ups, run:
* `git diff --output=..\..\..\..\scripts\patch\build.diff --ignore-cr-at-eol` from `\build\v8build\v8\build\`.
* `git diff --output=..\..\..\scripts\patch\src.diff --ignore-cr-at-eol` from `\build\v8build\v8\`.
* `git diff --output=..\..\..\..\..\scripts\patch\zlib.diff --ignore-cr-at-eol` from `\build\v8build\v8\third_party\zlib\`.

## Contributing
See [Contributing guidelines](./docs/CONTRIBUTING.md) for how to setup your fork of the repo and start a PR to contribute to React Native V8 JSI adapter.

## License

The V8 JSI adapter, and all newly contributed code is provided under the [MIT License](LICENSE). Portions of the JSI interface derived from Hermes are copyright Facebook.

## Code of Conduct

This project has adopted the [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/). For more information see the [Code of Conduct FAQ](https://opensource.microsoft.com/codeofconduct/faq/) or contact [opencode@microsoft.com](mailto:opencode@microsoft.com) with any additional questions or comments.
