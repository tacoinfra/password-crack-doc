---
title: How to install password recovery software on MacOS
description: How to install John the Ripper and other software components on MacOS.
date: 2026-05-14
author: Craig Buckler
---

# How install password recovery software on MacOS

Precompiled versions of John the Ripper are [available for MacOS](https://github.com/openwall/john-packages/releases/latest), and you can also install it using `brew install john-jumbo`. However, compiling the application on your local system provides better performance.

```bash
cd ~

# install dependencies
brew install openssl libomp gmp zlib

# clone latest bleeding-jumbo repository
git clone https://github.com/openwall/john -b bleeding-jumbo

# configure
cd john/src
./configure --with-openssl=$(brew --prefix openssl)

# compile
make -sj$(sysctl -n hw.ncpu)

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

Finally, you can permanently update your `PATH` environment variable if you want to use JtR regularly by adding a line to your `~/.bash_profile` shell configuration file, e.g.

```bash
# add at end of ~/.bash_profile
export PATH=$PATH:~/john/run
```

This tutorial presumes you can run `john` from anywhere. Test it works:

```bash
cd ~
john --list=build-info
```


## GPU support

JtR communicates with your graphics card using OpenCL (Open Computing Language).Apple M chips support OpenCL so there should be nothing to install or configure.

Check whether JtR can access your GPUs:

```bash
john --list=opencl-devices
```

You will either see a list of devices or an error such as *"Error: No OpenCL-capable devices were detected"*. Test GPU performance by entering:

```bash
john --test --format=tezos-opencl
```
