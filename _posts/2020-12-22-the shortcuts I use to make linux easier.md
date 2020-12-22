---
layout: post
title:  "The shortcuts I use to make linux easier"
categories: linux general
---

# The shortcuts I use to make linux easier
{: style="text-align: center"}

# Contents
- [**Introduction**](#introduction)<br>
- [**Using the command line**](#Using-the-command-line)<br>
   - [Reverse search for commands previously run](#requirements)<br>
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
   - [Jump to end of line](#jump-to-end-of-line)<br>
   - [Search for string](#search-for-string)<br>
- [**Managing processes**](#managing-processes)<br>
   - [Run a process in the background](#run-a-process-in-the-background)<br>
   - [Following a file](#following-a-file)<br>

<br>

# Introduction

Hello. This is a list that I've written off the top of my head. There are other useful commands I'm sure. I use these on a daily basis and it's helpful to take a look and think, should I really be using that? The `$` shows that these should be run as root. But so far looking at the list, none of them require that priviledge.

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

To go to the left..
```
$ ctrl <left arrow key>
```
To go to the right..
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
dd
```

**Jump to end of line**

```
$
```

And to get to the start of a line use `0`.

**Search for string**

Type `/` then your string and hit **`return`**. The move to the next result with `n` and the previous result with `N`.

# Processes

**Run a process in the background**<br>
Run this command while n the process stream..
```
$ ctrl z
```
Bring to foreground with `fg` `<job id>`.

**Following a file**
```
$ tail -f <file>
```
_"Output appended data as the file grows"_. Nice.
   
---

Thanks.
