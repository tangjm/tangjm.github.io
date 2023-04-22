---
title: 'Real world use case for the '--' argument for CLI programs'
date: '2023-04-22'
---

Ever since learning about the '--' command-line argument, I haven't found practical use cases for how it might be used other than for cases where you want to pass arguments that syntactically look like command-line arguments or options such as filenames like '-file1' to command-line programs.

To see why it might be needed for this purpose, consider the following:

```bash
$ cat --cities
cat: unrecognized option '--cities'
Try 'cat --help' for more information.
```

This can be fixed by running the following instead:

```bash
$ cat -- --cities
```

Everything after '--' is interpreted as an argument. We can think of this as the escape character for command-line flags and options. Just like how '\\' interprets '\' literally, `command -- -flag --option` interprets '-flag' and '--option' as ordinary arguments to `command` despite them having the '-' prefix.

Recently, I've found a more interesting and useful scenario for using the '--' argument. Many programs like 'curl', 'grep', 'mysql' and many others have a long list of options and command-line flags that alter program behaviour in a variety of ways.

It turns out that using '--' can help us filter the output from '--help' to substantially narrow down the documentation for a command like 'curl'.

```bash
# Schema for using the '--' argument.
# <your-command> --help | grep -- <your-flag-or-option>

# This will check how the '-O' flag affects the 'curl' command
curl --help | grep -- -O

# What does the '-h' flag do to the output of 'ls'?
ls --help | grep -- -h

# Ordinary command-line arguments and flags can also be used with '--', they just need to come first to prevent them from being interpreted as arguments to the command.
# This prints some lines of context before and after looking up how the '-P' flag affects the 'grep' command.
grep --help | grep -B 1 -A 2 -- -P
```

The next time you need to lookup what a specific flag or option does to a command, you can try this approach. 

