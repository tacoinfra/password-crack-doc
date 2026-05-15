---
title: How to run John the Ripper to recover your Tezos password
description: A description of various John the Ripper execution strategies to recover your Tezos password.
date: 2026-05-14
author: Craig Buckler
---

# How to run John the Ripper to recover your Tezos password

This section describes four strategies for running John the Ripper depending on what you know about your password.

At this point, you should have:

1. installed John the Ripper on [Windows](02_windows-install.md), [Linux](03_linux-install.md), or [MacOS](04_macos-install.md)

1. know whether you have a supported GPU by running `john --list=opencl-devices`

1. created a `tezos` working directory, and

1. generated a `hashes` file inside that directory.

`cd` to your `tezos` folder and ensure you can run `john` by either setting your `PATH` environment variable or using its fully-qualified path.


## Quick run

You can run JtR with the following command -- *but don't do it yet!*

```bash
john [options] hashes
```

JtR offers dozens of options. View the help by entering `john --help`.

The simplest way to start password recovery is:

```bash
john hashes
```

JtR attempts quick wins such as combinations of your user name and common passwords. It then starts incremental recovery where it attempts millions of combinations of characters.

Press space to show the current status:

```txt
0g 0:00:00:22 0.01% 2/3 (ETA: 2026-05-18 07:04) 0g/s 21937p/s 21937c/s 21937C/s goodday12..291052
```

The displays the:

* total processing time
* percentage complete
* processing step
* estimated time of completion, and
* information about the number of password checks per second.

For now, press `q` or `Ctrl`|`Cmd` + `C` to stop processing.


### Using your GPU

If you have a supported OpenCL GPU (see output of `john --list=opencl-devices`), adding the option `--format=tezos-opencl` to the `john` command will increase processing performance by offloading processing to that device.

If your PC has more than one graphics card, you can target a specific device or devices using a comma-delimited list, e.g. `--devices=1` or `--devices=1,3`. Device 1 is the first GPU shown in the output of `john --list=opencl-devices`.


### Disabling logs

As JtR runs, it outputs a large log to `john.log`. The log is not useful to JtR users, so disable it using the `--no-log` option.


### Limiting characters

Setting character limits can cut processing time. Use:

* `--length=N` to set the exact number of characters, e.g. `--length=12`
* `--min-length=N` to set a minimum number of characters, e.g. `--min-length=10`
* `--max-length=N` to set a maximum number of characters, e.g. `--max-length=15`

You can set both `--min-length` and `--max-length`.


### Saving and restoring sessions

JtR can save progress to a `.rec` file so you can stop and restore a session without having to restart processing from scratch. This means you can reboot your PC or run JtR when you're not using your device (JtR can run for a long time using significant CPU and/or GPU time).

To save session progress, set the `--session=NAME` option, e.g. `--session=tezos`. A `tezos.rec` file is then saved to your working directory.

You can examine the status of a running process with:

```bash
john --status=tezos
```

If you stop the session by pressing `q` or `Ctrl`|`Cmd` + `C`, you can restart it again using:

```bash
john --restore=tezos
```


### Example command

From your `tezos` working directory, start a quick run on a GPU, without logs, using a session named `tezos`, looking for a password between 7 and 9 characters, using the `hashes` file:

```bash
john --format=tezos-opencl --no-log --session=tezos --min-length=7 --max-length=9 hashes
```

Press `q` or `Ctrl`|`Cmd` + `C` to stop processing. To restart, enter:

```bash
john --restore=tezos
```

A quick run may be successful if you have a short password based on your name or a commonly-used word. However, [masking](#masking-run), [word list](#word-list-run), and [PRINCE mode](#prince-mode-run) runs offer a better chance of recovery.


## Masking run

You may use specific techniques or patterns when devising passwords. For example, you generally use:

1. Your dog's name: Rover. But it could be `rover`, `Rover`, or `R0ver`.
1. A special character, such as `!`, `$`, `&` etc.
1. A four-digit year which could start with `19` or `20`.
1. Two uppercase letters.

JtR provides a `--mask` option that allows you to define a template using [special character codes](https://github.com/openwall/john/blob/bleeding-jumbo/doc/MASK). For the password above, you'd use:

```bash
--mask=[Rr][o0]ver?s[12][90]?d?d?u?u
```

where:

* `[...]` defines a range or choice of letters: `R` or `r`, followed by `o` or `0`
* `?s` defines a special character that's not a letter or digit
* `?d` defines a digit
* `?u` defines an uppercase character
* other characters are as-is: `ver`

JtR can use this pattern to create a list of all possibilities, including:

```txt
Rover!1900AA
Rover#1900AB
Rover*1901XX
R0ver&2000YY
r0ver-2099ZZ
```

All candidates would be 12 characters because that's what the `mask` specifies. If you set `--max-length=13`, JtR repeats the last character type. As well as testing `Rover!1900AA`, JtR would also test `Rover!1900AAA`, `Rover!1900AAB`, and so on.

Example command for a masking run:

```bash
john --format=tezos-opencl --no-log --session=tezos --mask=[Rr][o0]ver?s[12][90]?d?d?u?u hashes
```

> Note: omit `--format=tezos-opencl` if you do not have a compatible GPU.


## Word list run

You may have a list of passwords you typically use, such as family names and weekdays, e.g.

```txt
anne
bob
chris
monday
tuesday
wednesday
thursday
friday
saturday
sunday
```

Save these to a file in your working directory named `wordlist.txt`. You can then pass it to `john` using the `--wordlist=wordlist.txt` option.

On it's own, JtR will examine those 10 words for an exact match. Adding the `--rules` option mutates all words by adding capitalization, making symbol replacements, duplicating words, and appending common letters, symbols, or digits. This considerably expands the candidates to include:

```txt
bob
bOb
boB
bOB
b0b
Bob
BOb
BoB
bob123
BOB!
bobbob
```

Example command using a mutated `wordlist.txt` wordlist to find a password between 6 and 9 characters:

```bash
john --format=tezos-opencl --no-log --session=tezos --wordlist=wordlist.txt --rules --min-length=6 --max-length=9 hashes
```

> Note: omit `--format=tezos-opencl` if you do not have a compatible GPU.


## PRINCE mode run

You may generate your passwords using certain names, symbol combinations, specific years, or other string snippets, e.g.

```txt
!$
$!
123
1234
12345
1967
1969
1999
2005
ANNE
Anne
BOB
Bob
CHRIS
Chris
anne
bob
chris
```

Save these to a file in your working directory named `wordpart.txt` and sort lines into ascending ASCII order (most editors provide that option or you can use an [online tool](https://www.textfixer.com/tools/sort-lines-alphabetically-online.php)).

JtR's PRINCE (PRobability INfinite Chained Elements) mode takes items from a wordlist and generates candidates by concatenating them in every possible order. The possibilities would include:

```txt
Anne!$
anne123
Bob!$1969
CHRIS123!$bob
1231967$!
```

Example command using a `wordpart.txt` wordlist with up to 4 string combinations to find a password between 8 and 12 characters:

```bash
john --format=tezos-opencl --no-log --session=tezos --prince=wordpart.txt --prince-elem-cnt-max=4 --min-length=8 --max-length=12 hashes
```

> Note: omit `--format=tezos-opencl` if you do not have a compatible GPU.

The `--prince-elem-cnt-max=N` option specifies the maximum number of string combinations. A 300 line wordlist with up 4 combinations typically takes a couple of days to process. Increasing it to 5 exponentially increases the number of candidates and processing could take weeks.


## `john.pot` recovery file

JtR creates a `john.pot` file when it starts processing. The file has no content until it recovers your password.

> Note: you can change the recovery filename using the `--pot` option, e.g. `--pot=recovery.pot`

On recovery, the `john.pot` file has the same content as your `hashes` file, followed by a colon (`:`) and your password. For example:

```txt
your@email.com:$tezos$1*2048*tezos secret seed phrase used to create your wallet that stores coins for spending later*your@email.com*tz1XXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXXX*abcdef0123456789fedcba9876543210cebcebcebceb:Password123
```

In this case, the recovered password is `Password123`.

> Note: do **not** share your `john.pot` file with anyone!


## More recovery options

If the suggested strategies do not work for you, John the Ripper has many more password recovery options.

* [John the Ripper home page](https://www.openwall.com/john/)
* [John the Ripper documentation](https://www.openwall.com/john/doc/)

This tutorial uses the *bleeding-jumbo* release which provides community enhancements:

* [JtR bleeding-jumbo on Github](https://github.com/openwall/john/tree/bleeding-jumbo)

You can use a wordlist generator to create password candidate files from a smaller list of suggestions:

* [Passwords generator](https://weakpass.com/tools/passgen)
* [Custom Wordlist Generator](https://password-strength-analyzer-tool.vercel.app/)
* [wgen.io](https://app.wgen.io/)
* [COOK](https://github.com/glitchedgitz/cook)
* [GENOVEVA](https://github.com/joseaguardia/GENOVEVA)
