---
layout: post
title:  "How To Debug Crashes And Segfaults"
categories: kernel
---

# How To Debug Crashes And Segfaults
{: style="text-align: center"}

Written by Nick Otter.

# Contents

- [**Introduction**](#introduction)<br>
- [**Ulimit**](#ulimit)<br>
     - [How to configure ulimit](#configure-ulimit)<br>
     - [How to test core dump file creation](#how-to-test-core-dump-file-creation)<br>
- [**How to read the core dump file**](#how-to-read-the-core-dump-file)
     - [backtrace](#backtrace)

# Introduction

When an application crashes or a segfault occurs, how can this be investigated? The answer is by core dump.

# N.B.

I recommend core dump use only on a case by case basis to debug. Persistent configuration leads to disk space hogging and security vulnerabilities as secrets can be exposed in the memory capture files.

A core dump is a memory dump of a program terminated by the operating system due to a fault. A typical reason is due to pointer errors - a program trying to access memmory it's not allowed to or doesn't have access to, causing a kill signal to be sent by the os. These errors are known as segfaults.

# Requirements

| Updated | `04/2020` | 
| Linux | `Kernel 5.4` `RHEL 8 4.18` |

# How to configure ulimit

Let's get started. `ulimit` will need to be configured to increase resource limits on this system. Using the `-c` argument will allow use to see the current core file creation limits. By default, core file creation is set to 0 to save on system performance.

1. Check current core dump file size settings:
```
$ ulimit -c
0
```

2. Set to a greater size of `0` by passing a size value with the `-c` flag. In this case, I'm happy to go unlimited for the fun of it.
```
$ ulimit -c unlimited
```  

3. Remove any conflicting `ulimit` configuration in persistent configuration files:
```
/etc/security/limits.conf
/etc/init.d/functions
/etc/profile
```

4. Set core dump file path and naming convention (see core [man page](http://man7.org/linux/man-pages/man5/core.5.html)).
```
$ echo "/tmp/core-%e.%p.%h.%t" > /proc/sys/kernel/core_pattern
```

| `/tmp/core-`| `%e`                 | `%p`                   | `%h`     | `%t` |
| Path.       | Executable filename. | PID of dumped process. | Hostname. | Time of dump. |

# How to test core dump file creation

Now, let's test core dump file creation by sending a segfault signal to a running process. 
```
$ killall -SIGSEV firefox
```

Check core dump file path.
```
[root@rhel-8-1 tmp]# sysctl kernel.core_pattern
kernel.core_pattern = /tmp/core-%e.%p.%h.%t
```

Check dump file has been created at path.
```
[root@rhel-8-1 tmp]# grep -l core /tmp/* 2>/dev/null | xargs ls -l
-rw-------. 1 root root 605036544 May 18 12:49 /tmp/core-firefox.6804.rhel-8-1.1589802596
```

# How to read the core dump file

Core dump files can be read with `gdb`. Let's read the one that was just created. For better info and not to face loads of `?????` in the `gdb` trace, install the `debug` version of the binary (and any shared libraries the binary uses) that caused the core dump. In this case I installed the package `firefox-debuginfo.x86_64`, the binary that caused the core dump was `firefox`.

With gdb, we'll be able to see the stack trace of the program and where the exception was raised that caused the core dump. Let's see if we can trace it through.

```
[root@rhel-8-1 tmp]# gdb `which firefox` core-firefox.6804.rhel-8-1.1589802596
```
```
Core was generated by `/usr/lib64/firefox/firefox'.
Program terminated with signal SIGSEGV, Segmentation fault.
#0  0x00007f366682ec5f in raise () from /lib64/libpthread.so.0
[Current thread is 1 (Thread 0x7f3666c49740 (LWP 6804))]
```

Ok. let's break this down

| `Core was generated by '/usr/lib64/firefox/firefox'.` |
|--
| Straight forward enough. Path to the binary that caused the core dump. |

| `Program terminated with signal SIGSEGV, Segmentation fault.` |
|--
| Great. We've found out what signal terminated the process.|

| `#0` | `0x00007f366682ec5f` | `raise ()` | `from /lib64/libpthread.so.0` |
|--    |--                    |--          |--                         
| Frame number (`#0` is executing frame at which dump occured, `#1` onwards are all frames that lead to dump). | Program counter/Instruction pointer/Memory address of where instruction was executed. | Function where the fault came from. | Script the function was in (`libpthread.so.0` is a shared library script that binary's use) (usually line numbers are shown here too, as it's a shared library function, no such luck, but this might be due to debug settings).| 

| `[Current thread is 1 (Thread 0x7f3666c49740 (LWP 6804))]`|
|--
| Thread memory address and `Light Weight Process` / thread ID of this gdb session. In gdb you can actually switch between sessions using this LWP ID. |

Ok. So.. on first look. If the program failed on the `shared library` function `raise ( )` then we could make an assumption that it's failed gracefully? As opposed to being a bug. Let's investigate further.  

Let's take a closer look at some of the backtrace `bt`. Backtrace shows all the executed frames leading up to the currently executing frame (`#0`), which is where the error occurred. Using the debug binary we should be able to see the functions called leading up to the crash, the arguments passed to them and their respective line number and source file. 

# backtrace

```
(gdb) bt
#0  0x00007f366682ec5f in raise () from /lib64/libpthread.so.0
#1  0x00007f36564b5238 in nsProfileLock::FatalSignalHandler (signo=11, 
    info=0x7ffed777e870, context=0x7ffed777e740)
    at /usr/src/debug/firefox-68.4.1-1.el8_1.x86_64/toolkit/profile/nsProfileLock.cpp:165
#2  0x00007f3656bef6e2 in WasmTrapHandler (signum=11, info=0x7ffed777e870, 
    context=0x7ffed777e740)
    at /usr/src/debug/firefox-68.4.1-1.el8_1.x86_64/js/src/wasm/WasmSignalHandlers.cpp:962
#3  <signal handler called>
#4  0x00007f366682a47c in pthread_cond_wait@@GLIBC_2.3.2 ()
   from /lib64/libpthread.so.0
#5  0x0000557d224171db in mozilla::detail::ConditionVariableImpl::wait (
    this=0x7f36657ab340, lock=...)
    at /usr/src/debug/firefox-68.4.1-1.el8_1.x86_64/mozglue/misc/ConditionVariable_posix.cpp:109
#6  0x00007f365343e415 in mozilla::ThreadEventQueue<mozilla::PrioritizedEventQueue<mozilla::EventQueue> >::GetEvent (this=0x7f36657ab2e0, aMayWait=true, 
    aPriority=0x7ffed777ee68)
    at /usr/src/debug/firefox-68.4.1-1.el8_1.x86_64/objdir/dist/include/mozilla/CondVar.h:57
#7  0x00007f3653440715 in nsThread::ProcessNextEvent(bool, bool*) ()
    at /usr/src/debug/firefox-68.4.1-1.el8_1.x86_64/objdir/dist/include/mozilla/
    --Type <RET> for more, q to quit, c to continue without paging--
```
Great... more info, also looks like some `firefox` source files paths have appeared too. Frame `#1` in the lead up to the crash supports that the process failed gracefully. `FatalSignalHandler` and seeing a couple of arguments `signo=` and `signum` both with the value of `11`* . From looking at this backtrace, we can assume that the `firefox` binary has a function to handle receiving a kill signal, and we could assume that the signal number sent to this function was `11` ? A bug might not look like this, `FatalSignalHandler` is a pretty appropriate function name to find one frame before a crash - probably isn't a crash (check out Brendan Gregg's [GDB Debugging: ncurses](http://www.brendangregg.com/blog/2016-08-09/gdb-example-ncurses.html) as a nice example of finding a bug with gdb). It seems like the segfault must have been caused by something else (e.g. us sending it to the process, which we know is the case). Let's see if in gdb we can actually look at that source code for `FatalSignalHandler` and continue.

* A quick look at the Linux `signal` [man page](http://man7.org/linux/man-pages/man7/signal.7.html) the signal number for a segfault is... **`11`**.

With `disas` we can see the dump of assembler code for a specific funtion in the trace (wow). In this assembler code dump, we are looking for a `=>` before the memory address - this marks the segmentation fault if there is one.

Right, let's check it out for `raise`:
```
(gdb) disas raise
Dump of assembler code for function raise:
   0x00007f366682eb50 <+0>:	endbr64 
   0x00007f366682eb54 <+4>:	push   %rbx
   0x00007f366682eb55 <+5>:	mov    $0xffffffffffffffff,%r8
   0x00007f366682eb5c <+12>:	mov    %edi,%ebx
   0x00007f366682eb5e <+14>:	mov    $0x8,%r10d
   0x00007f366682eb64 <+20>:	xor    %edi,%edi
   0x00007f366682eb66 <+22>:	sub    $0x110,%rsp
   0x00007f366682eb6d <+29>:	mov    %fs:0x28,%rax
   0x00007f366682eb76 <+38>:	mov    %rax,0x108(%rsp)
   0x00007f366682eb7e <+46>:	xor    %eax,%eax
   0x00007f366682eb80 <+48>:	mov    %rsp,%r9
   0x00007f366682eb83 <+51>:	movabs $0xfffffffe7fffffff,%rax
   0x00007f366682eb8d <+61>:	mov    %r8,0x88(%rsp)
   0x00007f366682eb95 <+69>:	mov    %rax,0x80(%rsp)
   0x00007f366682eb9d <+77>:	mov    %r9,%rdx
   0x00007f366682eba0 <+80>:	lea    0x80(%rsp),%rsi
   0x00007f366682eba8 <+88>:	mov    $0xe,%eax
   0x00007f366682ebad <+93>:	mov    %r8,0x90(%rsp)
   0x00007f366682ebb5 <+101>:	mov    %r8,0x98(%rsp)
   0x00007f366682ebbd <+109>:	mov    %r8,0xa0(%rsp)
   0x00007f366682ebc5 <+117>:	mov    %r8,0xa8(%rsp)
   0x00007f366682ebcd <+125>:	mov    %r8,0xb0(%rsp)
--Type <RET> for more, q to quit, c to continue without paging--
```

And then for `FatalSignalHandler` (parent being `nsProfileLock`):
```
(gdb) disas nsProfileLock::FatalSignalHandler
Dump of assembler code for function nsProfileLock::FatalSignalHandler(int, siginfo_t*, void*):
   0x00007f36564b5130 <+0>:	endbr64 
   0x00007f36564b5134 <+4>:	push   %r13
   0x00007f36564b5136 <+6>:	mov    %rdx,%r13
   0x00007f36564b5139 <+9>:	push   %r12
   0x00007f36564b513b <+11>:	mov    %rsi,%r12
   0x00007f36564b513e <+14>:	push   %rbp
   0x00007f36564b513f <+15>:	mov    %edi,%ebp
   0x00007f36564b5141 <+17>:	push   %rbx
   0x00007f36564b5142 <+18>:	lea    0x3650ce7(%rip),%rbx        # 0x7f3659b05e30 <_ZN13nsProfileLock12mPidLockListE>
   0x00007f36564b5149 <+25>:	sub    $0x98,%rsp
   0x00007f36564b5150 <+32>:	mov    %fs:0x28,%rax
   0x00007f36564b5159 <+41>:	mov    %rax,0x88(%rsp)
   0x00007f36564b5161 <+49>:	xor    %eax,%eax
   0x00007f36564b5163 <+51>:	jmp    0x7f36564b5178 <nsProfileLock::FatalSignalHandler(int, siginfo_t*, void*)+72>
   0x00007f36564b5165 <+53>:	nopl   (%rax)
   0x00007f36564b5168 <+56>:	cmpb   $0x0,0x10(%rdi)
   0x00007f36564b516c <+60>:	je     0x7f36564b517b <nsProfileLock::FatalSignalHandler(int, siginfo_t*, void*)+75>
   0x00007f36564b516e <+62>:	mov    $0x1,%esi
--Type <RET> for more, q to quit, c to continue without paging--
```

Hmmm no `=>` anywhere in sight. Does this mean there was no segfault inside the program stack? Yes! But for the sake of argument, let's say there was segfault (`=>`) at frame `<+51>`. 

```
   0x00007f36564b5163 <+51>:	jmp    0x7f36564b5178 <nsProfileLock::FatalSignalHandler(int, siginfo_t*, void*)+72>
```

We can see the `jmp` assembler instruction, and we can see the binary function `FatalSignalHandler` that caused the (imaginary) segfault. If we want to do further investigation, we can take a look at the source code of the binary the function was called in within `gdb`, side by side to the trace with `disas/s`.

Let's take a look at the source code that was run at frame `<+51>`. This will be the source code of where our imaginary segfault came from. Also, scrolling down to frame `<+189>` of the trace, we can see the source code to handle the `SIGSEGV` signal we sent at line 58 of this article which includes a `break`.

Look for **`<+51>`** and **`<189>`** (`nopl` at frame 53 is just an assembler function for do nothing...). We can now see the source code after `<+51>` as it was run.

```
$ (gdb) disas/s nsProfileLock::FatalSignalHandler
```

```
118	  RemovePidLockFiles(true);
   0x00007f36564b5163 <+51>:	jmp    0x7f36564b5178 <nsProfileLock::FatalSignalHandler(int, siginfo_t*, void*)+72>
--Type <RET> for more, q to quit, c to continue without paging--<RET>
   0x00007f36564b5165 <+53>:	nopl   (%rax)

180	}
181	
182	nsresult nsProfileLock::LockWithFcntl(nsIFile* aLockFile) {
183	  nsresult rv = NS_OK;
184	
185	  nsAutoCString lockFilePath;
186	  rv = aLockFile->GetNativePath(lockFilePath);
187	  if (NS_FAILED(rv)) {
188	    NS_ERROR("Could not get native path");
189	    return rv;
190	  }
```

```
139	    case SIGSEGV:
140	      oldact = &SIGSEGV_oldact;
141	      break;
   0x00007f36564b51e6 <+182>:	lea    0x36af553(%rip),%rsi        # 0x7f3659b64740 <_ZL14SIGSEGV_oldact>
   0x00007f36564b51ed <+189>:	jmp    0x7f36564b51a0 <nsProfileLock::FatalSignalHandler(int, siginfo_t*, void*)+112>
```
---

Thanks. [Example of using gdb and strace to find the cause of a segmentation fault](http://bl0rg.krunch.be/segfault-gdb-strace.html), [GDB Debugging: ncurses](http://www.brendangregg.com/blog/2016-08-09/gdb-example-ncurses.html) and other articles were useful to write this. This was written by Nick Otter.
