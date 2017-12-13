---
title: Maria GDK client configuration and troubleshooting
keywords: client installation troubleshooting
tags: [client, map, rendering]
sidebar: core_setup_sidebar
permalink: core_setup_client_configuration.html
summary: Configuring Maria GDK clients. 
---

The Maria client setup will mainly be application specific, but there are a few common settings that control the low level components of the system that will be relevant for all Maria applications.

## Command line options

All Maria GDK client applications will by default support a command line option to override the map rendering backend. This will work as long as no explicit rewrite of the application argument vector is done. The command line option is `--glmode <mode>` where `<mode>` can be one of the following:

* `angle` (default). Use the ANGLE OpenGL/Direct3D renderer. This option has the best performance, and should in general work for most platforms. For more control over the Direct3D backend, see the [GPU Blacklist](#gpu-blacklist) section below.
* `opengl` Use the desktop OpenGL driver. This is generally not recommended, but may be useful for certain corner cases. This will give full hardware acceleration in the map renderer, but involves a software buffer copy that may cause slowdowns, especially on higher resolution displays.
* `software` Pure software OpenGL rendering, with software frame buffer copy. This is the slowest option, but most compatible.

## GPU Blacklist

The map rendering core of Maria GDK uses the graphics processing unit, and may be sensitive to bugs and peculiarities in certain graphics card drivers. Therefore we support a configuration file that enables and disables certain features based on the GPU ID, Vendor and driver versions. This file should be named *`gl_blacklist.json`* and be located along with the .exe file.

This file is based on the [Qt OpenGL bug file](http://doc.qt.io/qt-5/windows-requirements.html#dynamically-loading-graphics-drivers) which utilizes the format of the driver bug list used in the [Chromium](http://www.chromium.org/Home) projects. It consists of a list of entries each of which specifies a set of conditions and a list of feature keywords. Typically, device id and vendor id are used to match a specific graphics card. They can be found in the output of the `dxdiag` tool. 

These values are also written to the client log file on DEBUG log level, for example: 

```
2017-09-29 14:25:27,903 [1] DEBUG MariaNativeRendering - GPU Info:
         Card name: Intel(R) HD Graphics 530
       Driver Name: igdumdim64.dll
    Driver Version: 21.20.16.4664
         Vendor ID: 0x8086
         Device ID: 0x191B
         SubSys ID: 0x06E51028
       Revision ID: 0x0006

```

The following feature keywords are relevant for choosing the OpenGL implementation:

* `disable_angle` - (Maria GDK 3.2 and newer) Disables ANGLE. Do not use ANGLE/Direct3D, use software OpenGL rendering instead. This should work on most platforms, but at a lower performance.
* `disable_d3d11` - Disables the D3D11 rasterizer in ANGLE. Instead, the next D3D rendering option is tried first. The default order is: D3D11, D3D9, WARP, reference implementation.
* `disable_d3d9` - Disables the D3D9 rasterizer in ANGLE (D3D11 may still be used if available.)


```json
{
"entries": [
{
  "id": 1,
  "description": "Disable D3D11 on older nVidia drivers",
  "vendor_id": "0x10de",
  "device_id": ["0x0DE9"],
  "driver_version": {
    "op": "<=",
    "value": "8.17.12.6973"
  },
  "features": [
    "disable_d3d11"
  ]
},
...
```

The supported keys for matching a given card or driver are the following. 

* `os.release` - Specifies a list of operating system releases on Windows: xp, vista, 7, 8, 8.1, 10.
* `vendor_id` - Vendor from the adapter identifier
* `device_id` - List of PCI device IDs.
* `driver_version` - Driver version from the adapter identifier
* `driver_description` - Matches when the value is a substring of the driver description from the adapter identifier
* `gl_vendor` - Matches when the value is a substring of the GL_VENDOR string
