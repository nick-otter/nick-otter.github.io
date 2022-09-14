---
layout: post
title:  "Oh OS, Operating System Introductions from Shlomi Boutnaru at Rezilion"
categories: solutions os
---

# Oh OS, Operating System Introductions from Shlomi Boutnaru at Rezilion
{: style="text-align: center"}

[Shlomi Boutnaru](https://www.linkedin.com/in/shlomi-boutnaru-ba781811a/) at [Rezilion](https://www.rezilion.com/) recently published ["Operating Systems: Part 1 - Introduction"](https://www.linkedin.com/posts/shlomi-boutnaru-ba781811a_operatingsystems-kernel-kernelmode-activity-6974947995058335744-YqrQ?utm_source=share&utm_medium=member_desktop).

Let's check it out. 

## Introduction

Generally, an OS (Operating System) is a piece of software that manages applications, users and the hardware resources of the computing device. Thus, an OS can provide isolation, communication and resources allocation - let us go over those activities. 

![](https://upload.wikimedia.org/wikipedia/commons/d/d0/OS-structure2.svg)

## User-mode, Kernel-mode
The generic operating systems that we are going to talk about for now (like Windows, Linux and OSX) are divided into two main parts: user-mode and kernel-mode (I am excluding for now security features such as VBS on Windows - they are going to be covered separately). 

## The Kernel 
The kernel is responsible for different tasks such as: memory management, interrupt handling, process/thread scheduling, networking and other IO devices management. There are different types of kernel: 

* **modular kernel**: load driver/kernel modules to a running kernel without the need to compile them into the kernel.
* **monolithic kernel**: the entire OS code resides in the kernel. 
* **microkernel**: only part of the OS code reside in the kernel, for example device drivers, networking stacks and filesystems don’t execute as part of the kernel.
* **hybrid kernel**: some kind of combination between monolithic and microkernel - for more context check out the illustration above.

## x86, rings
For now you should know that kernel mode and user mode are two different permission levels which are enforced by the hardware (CPU in our case). In x86 we have “rings” and in ARM we have “exceptions levels” - we will focus on x86 for now ARM will be covered in the future. 
In x86 we have 4 rings, we are going to talk mostly about “ring 0” (kernel) and “ring 3” (user-mode). 

## ring 0, ring 3
When running kernel code, the CPU is in “ring 0” (x86) and when running user-mode code it is running in “ring 3”. “ring 0” is the highest privilege level and it is the only ring which has the ability to perform I/O and access hardware directly. So, when an application running in user-mode wants to write to a file it needs the help of the kernel, it is done by syscalls (functions exposed by the kernel, which allow kernel code to run on behalf of a user-mode application). Most of the time you won’t call the syscall directly but rather you will call a wrapper (which is part of the programming language/runtime). 

Next, we are going to talk about processes and threads. See you soon. 

---

Thanks. Unsurprisingly, this article was not written by me but [Shlomi Boutnaru](https://www.linkedin.com/in/shlomi-boutnaru-ba781811a/) at [Rezilion](https://www.rezilion.com/).
