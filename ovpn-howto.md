# OpenVPN Howto

## Build `OVPN-DCO-Win`

[OpenVPN DCO Win](https://github.com/OpenVPN/ovpn-dco-win/blob/master/README.md)
[Windows Driver Kit(WDK)](https://learn.microsoft.com/en-us/windows-hardware/drivers/download-the-wdk)
[Windows 11 EWDK](https://learn.microsoft.com/en-us/windows-hardware/drivers/download-the-wdk)

### Download Windows 11 EWDK

```
As an alternative to downloading Visual Studio, the SDK, and the WDK, you
can download the EWDK, which is a standalone, self-contained command-line
environment for building drivers. It includes Visual Studio Build Tools,
the SDK, and the WDK.

Once you have downloaded the ISO, use these steps to set up your build environment:

  1. Mount the EWDK ISO from a drive volume. Network share paths aren't currently supported.
  2. Run LaunchBuildEnv.cmd.
  3. In the environment created in step 2, type SetupVSEnv, and then press Enter.
  4. Launch devenv.exe from the same environment, using the full file path. For example:
     "C:\Program Files\Microsoft Visual Studio\2022\%Community|Professional|Enterprise%\Common7\IDE\devenv.exe"
  5. When you're done with the build environment, you may want to eject the ISO.
```

### Build `ovpn-dco-win`

```
1. Mount the Windows 11 EWDK iso image.
2. Clone this repository and check out the version that your file is supposed to be.
3. In Powershell, navigate to the virtual drive and run LaunchBuildEnv.cmd.
4. Build the driver: msbuild /p:platform=<arch> /p:configuration=release /p:signmode=off ovpn-dco-win.vcxproj /t:Build where <arch> is Win32, x64, ARM or ARM64.
   msbuild /p:platform=x64 /p:configuration=release /p:signmode=off ovpn-dco-win.vcxproj /t:build
   msbuild /p:platform=win32 /p:configuration=release /p:signmode=off ovpn-dco-win.vcxproj /t:build
   msbuild /p:platform=arm64 /p:configuration=release /p:signmode=off ovpn-dco-win.vcxproj /t:build
   msbuild /p:platform=ARM /p:configuration=release /p:signmode=off ovpn-dco-win.vcxproj /t:build
5. Check that <arch>/Release/ovpn-dco.sys and the file from step 1 have the same SHA256 hash.
6. Remove the signature from the ovpn-dco.sys file you received: signtool remove /s ovpn-dco.sys
```
