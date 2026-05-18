---
title: How to generate your Tezos `hashes` file
description: How to generate your Tezos hashes file for John the Ripper recovery processing.
date: 2026-05-14
author: Craig Buckler
---

# How to generate a Tezos `hashes` file

John the Ripper requires a `hashes` file for password processing. It provides a `tezos2john.py` Python script in its `run` directory that generates this file from your Tezos information:

1. Your 15 seed words
1. Your email address (case sensitive)
1. Your public address (a 36-character string starting `tz`)

The command format is:

```bash
python3 tezos2john.py '<seed-words>' '<email>' '<tzAddress>' > hashes
```

Generating the `hashes` file is a one-off task. You can install Python locally or in a virtual machine, but using Docker may be easier.


## Generating your `hashes` file using Docker

If you do not have Python installed, you can run it from a [Docker](https://www.docker.com/) container. Docker Desktop is available for [Windows](https://docs.docker.com/desktop/setup/install/windows-install/), [Linux](https://docs.docker.com/desktop/setup/install/linux/), and [macOS](https://docs.docker.com/desktop/setup/install/mac-install/).

Once you have installed Docker Desktop:

1. Create a working directory such as `C:\tezos` on Windows or `~/tezos` on Linux or Mac OS.

1. `cd` to the John the Ripper `run` directory.

1. Run the following `docker` command on Linux or macOS, replacing the seed words, email, tz address, and `hashes` file location as necessary:

    ```bash
    # generate hash (Linux/macOS)
    docker run -it --rm --name python \
      -v ${PWD}:/usr/src/myapp \
      -w /usr/src/myapp \
      python:3-alpine \
      python tezos2john.py \
      'tezos secret seed phrase used to create your wallet that stores coins for spending later' \
      'your@email.com' \
      'tz1XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX' \
      > ~/tezos/hashes
    ```

    The equivalent Windows Powershell command:

    ```ps
    # generate hash (Windows)
    docker run -it --rm --name python -v ${PWD}:/usr/src/myapp -w /usr/src/myapp python:3-alpine python tezos2john.py 'tezos secret seed phrase used to create your wallet that stores coins for spending later' 'your@email.com' 'tz1XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX' > C:\tezos\hashes
    ```


## Generating your `hashes` file using Python

Some Linux distros provide Python by default and downloads are available for [Windows](https://www.python.org/downloads/windows/) and [macOS](https://www.python.org/downloads/macos/).

> Note: Windows and macOS users can use WSL2 or a Linux virtual machine. Copy `tezos2john.py` and the `bip-0039` subdirectory from the JtR `run` directory into the VM and run it using the command below.

Once you have installed Python:

1. Create a working directory such as `C:\tezos` on Windows or `~/tezos` on Linux or Mac OS.

1. `cd` to the John the Ripper `run` directory.

1. Run the following `python` command on Linux or macOS, replacing the seed words, email, tz address, and `hashes` file location as necessary:

    ```bash
    # generate hash (Linux/macOS)
    python3 tezos2john.py \
      'tezos secret seed phrase used to create your wallet that stores coins for spending later' \
      'your@email.com' \
      'tz1XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX' \
      > ~/tezos/hashes
    ```

    > Note: if this command fails, try `python` instead of `python3`.

    The equivalent Windows Powershell command:

    ```ps
    # generate hash (Windows)
    python tezos2john.py 'tezos secret seed phrase used to create your wallet that stores coins for spending later' 'your@email.com' 'tz1XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX' > C:\tezos\hashes
    ```


## Examine your `hashes` file

Assuming there were no errors, your generated `hashes` file contains your Tezos email, seed words, and address, with a hash value in a format similar to this example:

```txt
your@email.com:$tezos$1*2048*tezos secret seed phrase used to create your wallet that stores coins for spending later*your@email.com*tz1XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX*abcdef0123456789fedcba9876543210cebcebcebceb
```

> Note: do **not** share your `hashes` file with anyone!

You can now [proceed to password cracking...](06_start-jtr.md)
