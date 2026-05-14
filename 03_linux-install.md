---
title: How to install password recovery software on Linux
description: How to install John the Ripper and other software components on Linux.
date: 2026-05-14
author: Craig Buckler
---

# How install password recovery software on Linux

Although flatpak versions of John the Ripper are [available for Linux](https://github.com/openwall/john-packages/releases/latest), compiling the application on your local system provides better performance.

```bash
cd ~

# install dependencies
sudo apt-get install git build-essential libssl-dev zlib1g-dev yasm pkg-config libgmp-dev libpcap-dev libbz2-dev nvidia-opencl-dev

# clone latest bleeding-jumbo repository
git clone https://github.com/openwall/john -b bleeding-jumbo john

## build
cd john/src && ./configure && make -s clean && make -sj4

# test
cd ../run/ && ./john --list=build-info
```

For the purposes of this tutorial, we'll assume you've installed JtR to `~/john` and the `~/john/run` directory provides JtR executables. You can run `john` from the terminal using the full path, e.g.

```bash
~/john/run/john
```

Alternatively, you can update your `PATH` environment variable temporarily so `john` can be executed from anywhere:

```bash
# do after opening terminal
PATH=$PATH:~/john/run

# run john from any directory
cd ~
john
```

Finally, you can permanently update your `PATH` environment variable if you want to use JtR regularly by adding a line to your `~/.bashrc` or similar shell configuration file, e.g.

```bash
# add at end of ~/.bashrc
export PATH=$PATH:~/john/run
```

This tutorial presumes you can run `john` from anywhere. Test it works:

```bash
cd ~
john --list=build-info
```


## GPU support

JtR communicates with your graphics card using OpenCL (Open Computing Language) -- refer to the [Linux device support list](https://opencl.gpuinfo.org/listdevices.php?platform=linux). Distros and devices vary, but you can install the latest NVIDIA drivers on Ubuntu using:

```bash
ubuntu-drivers devices
sudo ubuntu-drivers install
sudo apt update
sudo apt install -y nvidia-opencl-dev clinfo
```

Check whether JtR can access your GPUs:

```bash
john --list=opencl-devices
```

You will either see a list of devices or an error such as *"Error: No OpenCL-capable devices were detected"*. Test GPU performance by entering:

```bash
john --test --format=tezos-opencl
```
