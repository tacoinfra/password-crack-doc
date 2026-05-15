---
title: How to install password recovery software on Windows
description: How to install John the Ripper and other software components on Windows.
date: 2026-05-14
author: Craig Buckler
---

# How to install password recovery software on Windows

A pre-compiled version of John the Ripper is available for Windows from [github.com/openwall/john-packages/releases/latest](https://github.com/openwall/john-packages/releases/latest).

Download and extract the file named `winX64_1_JtR.zip`. For the purposes of this tutorial, we'll assume you've extracted it to `C:\JtR`.

The `C:\JtR\run` directory provides JtR executables. You can run `john.exe` from Powershell using the full path, e.g.

```ps
C:\JtR\run\john
```

Alternatively, you can update your `PATH` environment variable temporarily to execute `john.exe` from anywhere:

```ps
# do after opening Powershell
$env:path += ";C:\JtR\run\"

# run john from any directory
john
```

Or you can permanently update your `PATH` environment variable if you intend using JtR regularly:

1. Click **Start** and type "environment variables" or choose **Start** > **Settings** > **System** > **About** > **Advanced system settings**.

1. In the **System Properties** dialog, click the "Environment Variables..." button.

1. Choose "Path" and click "Edit...".

1. Click "New", enter `C:\JtR\run\`, and click "OK".

This tutorial presumes you can run `john` from anywhere. Test it works:

```ps
cd \
john --list=build-info
```


## GPU support

JtR communicates with your graphics card using OpenCL (Open Computing Language) -- refer to the [Windows device support list](https://opencl.gpuinfo.org/listdevices.php?platform=windows). Windows generally installs the correct graphics card driver for your system, but newer devices may require a manual download.

Check whether JtR can access your GPUs:

```ps
john --list=opencl-devices
```

You will either see a list of devices or an error such as *"Error: No OpenCL-capable devices were detected"*. Test GPU performance by entering:

```ps
john --test --format=tezos-opencl
```

You can now [proceed to generating your hashes file...](05_generate-hashes.md)
