---
title: How to generate your Tezos `hashes` file
description: How to generate your Tezos hashes file for John the Ripper recovery processing.
date: 2026-05-14
author: Craig Buckler
---

# How to generate a Tezos `hashes` file

John the Ripper requires a `hashes` file that contains your Tezos information:

1. Your 15 seed words
1. Your email address (case sensitive)
1. Your public address (a 36-character string starting `tz`)

JtR provides a `tezos2john.py` Python script in its `run` directory that generates the hash from this information:

```bash
python3 tezos2john.py '<seed-words>' '<email>' '<tzAddress>'
```

Generating the hash is a one-off task. You can install Python, but using Docker is easier.


## Generating your `hashes` file using Docker

If you do not have Python installed, you can use [Docker](https://www.docker.com/) to run it from a container. Docker Desktop is available for [Windows](https://docs.docker.com/desktop/setup/install/windows-install/), [Linux](https://docs.docker.com/desktop/setup/install/linux/), and [MacOS](https://docs.docker.com/desktop/setup/install/mac-install/).

Once you have installed Docker Desktop:

1. Create a working directory such as `C:\tezos` on Windows or `~/tezos` on Linux or Mac OS.

1. `cd` to the John the Ripper `run` directory.

1. Run the following command on Linux or MacOS, replacing the seed words, email, tz address, and `~/tezos/hashes` location as necessary:

    ```bash
    # generate hash (Linux/MacOS example)
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
    # generate hash (Windows example)
    docker run -it --rm --name python -v ${PWD}:/usr/src/myapp -w /usr/src/myapp python:3-alpine python tezos2john.py 'tezos secret seed phrase used to create your wallet that stores coins for spending later' 'your@email.com' 'tz1XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX' > C:\tezos\hashes
    ```


## Generating your `hashes` file using Python

Some Linux distros provide Python by default and downloads are available for [Windows](https://www.python.org/downloads/windows/) and [MacOS](https://www.python.org/downloads/macos/).

> Note: you could use a Linux virtual machine or WSL2. Copy `tezos2john.py` and the `bip-0039` subdirectory from the JtR `run` directory into the VM and run it there.

Once you have installed Python:

1. Create a working directory such as `C:\tezos` on Windows or `~/tezos` on Linux or Mac OS.

1. `cd` to the John the Ripper `run` directory.

1. Run the following command on Linux or MacOS, replacing the seed words, email, tz address, and `hashes` location as necessary:

    ```bash
    # generate hash (Linux/MacOS example)
    python3 tezos2john.py \
      'tezos secret seed phrase used to create your wallet that stores coins for spending later' \
      'your@email.com' \
      'tz1XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX' \
      > ~/tezos/hashes
    ```

    > Note: try `python` instead of `python3` if the command fails.

    The equivalent Windows Powershell command:

    ```ps
    # generate hash (Windows example)
    python tezos2john.py 'tezos secret seed phrase used to create your wallet that stores coins for spending later' 'your@email.com' 'tz1XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX' > C:\tezos\hashes
    ```


## Examine your `hashes` file

Assuming there were no generation errors, your `hashes` file contains your Tezos data in a format similar to this example:

```txt
your@email.com:$tezos$1*2048*tezos secret seed phrase used to create your wallet that stores coins for spending later*your@email.com*tz1XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX*abcdef0123456789fedcba9876543210cebcebcebceb
```

> Note: do **not** share your `hashes` file with anyone!

You can now [proceed to password cracking...](06_start-jtr.md)
