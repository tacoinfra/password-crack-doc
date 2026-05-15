---
title: How to recover your forgotten Tezos password
description: Steps you can take to recover your forgotten Tezos password using John the Ripper.
date: 2026-05-14
author: Craig Buckler
---

# How to recover your forgotten Tezos password

Recovering your Tezos password is possible using a tool such as [John the Ripper](https://www.openwall.com/john/). It can attempt millions of combinations of characters to find the correct string.


## Your chances of password recovery

John the Ripper can check 100,000 passwords per second on a modest PC. A modern PC with fast GPUs could exceed 500,000 password checks per second.

This does not necessarily make recovery easy. Assume:

1. you have a fast dedicated PC which can handle 1 million password checks per second, and
1. your password uses a set of 90 characters including A-Z, a-z, 0-9, and symbols from a typical keyboard (!, $, %, &, +, etc.).

A 6-character password has 90<sup>6</sup> combinations -- a total of 531 billion possibilities. It would take more than 6 days to check them all, or perhaps 3 to 4 days to recover the *average* 6-character password.

Every appended character exponentially increases the number of password possibilities:

* 7 characters: 1 to 2 years
* 8 characters: 70 to 130 years
* 9 characters: up to 11,000 years
* 10 characters: up to 1 million years
* 11 characters: up to 100 million years
* 12 characters: 9 billion years - *twice the age of the Earth!*

Recovery is viable if your password is short (7 or fewer characters), common (`password`, `123456`, `qwerty` etc.), or you have some idea about its format and content.

John the Ripper or similar brute force tools will **never** recover a long random password set by your browser or a generation app.


## Requirements

You require a Windows, Linux, or Mac PC and must have the technical ability to install software and run command-line tools.

You require the following Tezos ICO information:

1. Your 15 seed words
1. Your email address (case sensitive)
1. Your public address (a 36-character string starting `tz`)

Optional requirements:

* one or more OpenCL-compatible graphics card such as those [supplied by NVIDIA, AMD, and Intel](https://opencl.gpuinfo.org/listdevices.php) or Apple M chipsets.
* some password hints, e.g. you think it's your name followed by some digits and a special character.

> Note: you can run password recovery tools in Windows WSL2, a virtual machine, or Docker container, but these are not the fastest options and require additional configuration to use the host's GPU. This tutorial recommends a bare-metal installation.


## Steps

This tutorial explains how to:

1. install John the Ripper and GPU drivers on [Windows](02_windows-install.md), [Linux](03_linux-install.md), and [MacOS](04_macos-install.md)

1. [generate the `hashes` file](05_generate-hashes.md) John the Ripper uses for processing, and

1. [start John the Ripper](06_start-jtr.md) using an appropriate recovery strategy.

Good luck.
