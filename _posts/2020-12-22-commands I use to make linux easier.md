---
layout: post
title:  "Commands I use to make Linux easier"
categories: linux
---

# Commands I use to make Linux easier
{: style="text-align: center"}

Written by Nick Otter.

# Contents
- [**Introduction**](#introduction)<br>
- [**Using the command line**](#Using-the-command-line)<br>
   - [Reverse search for commands run](#reverse-search-for-commands-run)<br>
   - [View history of commands run](#view-history-of-commands-run)<br>
   - [Run command in your history](#run-command-in-your-history)<br>
   - [Autocomplete command](#autocomplete-command)<br>
   - [Search manual pages](#search-manual-pages)<br>
   - [View manual page and search](#view-manual-page-and-search)<br>
 - [**Cursor love**](#cursor-love)<br>
   - [Moving..](#moving..)<br>
   - [Deleting..](#deleting..)<br>
- [**Using vim**](#using-vim)<br>
   - [Delete line](#delete-line)<br>
   - [Batch delete](#batch-delete)<br>
   - [Jump to end of line](#jump-to-end-of-line)<br>
   - [Jump to start of file](#jump-to-start-of-file)<br>
   - [Search for string](#search-for-string)<br>
- [**Managing processes**](#managing-processes)<br>
   - [Run a process in the background](#run-a-process-in-the-background)<br>
   - [Following a file](#following-a-file)<br>
- [**Executing commands**](#executing-commands)<br>
   - [Execute a command with multiple arguments](#execute-a-command-with-multiple-arguments)<br>
   - [Using process substitutions instead of piping](#using-process-substitutions-instead-of-piping)


# Introduction

Hello. This is a list that I've written off the top of my head. There are other useful commands too. I use these on a daily basis and it's helpful to take a look and think, should I really be using that? The `$` shows that these should be run as root. But so far looking at the list, none of them require that priviledge.

Thanks for reading. 

# Using the command line
**Reverse search for commands previously run**<br>
```
$ ctrl r <string in command you want to search for>
```

**View history of commands run**<br>
```
$ history
```

**Run command in your history**<br>
```
$ !<number>
```

**Autocomplete command**<br>
```
$ <tab>
```
(In `RHEL` and `CentOS` this package is known as `bash-completion` or `bash-completion-extras`).

**Search manual pages**<br>
```
$ man -k <what you are looking for>
```
(In `RHEL`, `CentOS` and `Ubuntu` run **`$ mandb`** to rebuild the manual database). 

**View manual page and search**<br>
```
$ man <category> <entry>
```
Then, when viewing the entry press **`/`** then type the string you are looking for and press **`Return`**. Press **`n`** to move to the next result and **`N`** to move before.

# Cursor love

**Moving..**

**Move cursor by word instead of letter**<br>

To the left..
```
$ ctrl <left arrow key>
```
and to the right..
```
$ ctrl <right arrow key>
```

**Move to start of line**<br>
```
$ ctrl a
```

And to the end..
```
$ ctrl e
```

**Deleting..**

Delete letter under cursor
```
$ ctrl d
```

Delete all words after cursor
```
$ ctrl k
```

Delete word before cursor
```
$ ctrl w
```

# Using vim

(All commands to be run _not_ in `insert` mode.)

**Delete line**

```
Esc + dd
```

**Batch delete**

Enter visual mode:
```
Esc + ctrl v
```
Get deleting:
```
d
```

**Jump to end of line**

```
$
```

And to get to the start of a line use `0`.

**Jump to start of file**

```
Esc + gg
```

And to get to the bottom of file, use `Esc + G`.


**Search for string**

Type `/` then your string and hit **`return`**. The move to the next result with `n` and the previous result with `N`.

# Processes

**Run a process in the background**<br>
Run this command while in the process stream..
```
$ ctrl z
```
Bring to foreground with `fg` `<job id>`.

**Following a file**
```
$ tail -f <file>
```
_"Output appended data as the file grows"_. Nice.

# Executing commands

**Execute a command with multiple arguments**
```
$ <command> {arg,arg}
```
E.g.
```
$ ls {dir_1,dir_2}
```

**Using process substitutions instead of piping**
```
$ <command> <(output of other command to pass to first command) <(output of other command to pass to first command)
```
E.g. Diff the contents of `file_1` `file_2` after they have been sorted by `sort`.
```
$ diff <(sort file_1) <(sort file_2)
```
With process substitutions, arguments are passed with `/dev/fd/63` not `stdin` like pipe.

---

Thanks. This was written by Nick Otter.
