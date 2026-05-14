---
title: How to generate a Tezos `hashes` file
description: How to generate a Tezos hashes file for John the Ripper recovery processing.
date: 2026-05-14
author: Craig Buckler
---

# How to generate a Tezos `hashes` file

John the Ripper requires a `hashes` file that contains your Tezos information:

1. Your 15 seed words
1. Your email address (case sensitive)
1. Your public address (a 36-character string starting `tz`)

JtR provides a `tezos2john.py` Python script in the `run` directory that generates the file from this information:

```bash
python3 tezos2john.py '<seed-words>' '<email>' '<tzAddress>'
```


## Generating your `hashes` file using Docker

If you do not have Python installed, it's easiest to use [Docker](https://www.docker.com/) and run it from a container. Docker Desktop is available for [Windows](https://docs.docker.com/desktop/setup/install/windows-install/), [Linux](https://docs.docker.com/desktop/setup/install/linux/), and [MacOS](https://docs.docker.com/desktop/setup/install/mac-install/).

Once Docker Desktop is installed:

1. Create a working directory such as `C:\tezos` on Windows or `~/tezos` on Linux or Mac OS.

1. `cd` to the John the Ripper `run` directory.

1. run the following command, replacing the seed words, email, tz address, and `hashes` location as necessary:

    ```bash
    # generate hash (example)
    docker run -it --rm --name python \
      -v ${PWD}:/usr/src/myapp \
      -w /usr/src/myapp \
      python:3-alpine \
      python tezos2john.py \
      'tezos secret seed phrase used to create your wallet that stores coins for spending later' \
      'your@email.com' \
      'tz1XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX' \
      > ~/tezos/hashes # C:\tezos\hashes on Windows
    ```

    > Note: this example uses `\` line breaks for readability. Remove them and paste a single line into Windows Powershell.


## Generating your `hashes` file using Python

Some Linux distros provide Python by default and it's available for [Windows](https://www.python.org/downloads/windows/) and [MacOS](https://www.python.org/downloads/macos/).

Once Python is installed:

1. Create a working directory such as `C:\tezos` on Windows or `~/tezos` on Linux or Mac OS.

1. `cd` to the John the Ripper `run` directory.

1. run the following command, replacing the seed words, email, tz address, and `hashes` location as necessary:

    ```bash
    # generate hash (example)
    python3 tezos2john.py \
      'tezos secret seed phrase used to create your wallet that stores coins for spending later' \
      'your@email.com' \
      'tz1XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX' \
      > ~/tezos/hashes # C:\tezos\hashes on Windows
    ```

    > Note: this example uses `\` line breaks for readability. Remove them and paste a single line into Windows Powershell.


## Examine your `hashes` file

Assuming you had no errors, the `hashes` file contains your Tezos data in a format similar to below:

```txt
your@email.com:$tezos$1*2048*tezos secret seed phrase used to create your wallet that stores coins for spending later*your@email.com*tz1XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX*abcdef0123456789fedcba9876543210cebcebcebceb
```

> Note: **never** share your `hashes` file with anyone!

You can now [proceed to password cracking...](06_start-jtr.md)
