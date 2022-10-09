---
layout: post
title:  "How to debug an emergency boot"
categories: linux kernel boot
---

# How to debug an emergency boot
{: style="text-align: center"}

Written by Nick Otter.

# Contents

- [**Introduction**](#introduction)<br>
  - [Boot procedure explained](#boot-procedure-explained)<br>
  - [Requirements](#requirements)<br>
- [**Emergency target vs. rescue target**](#emergency-target-vs.-rescue-target)<br>
- [**How to discover what target you are in**](#how-to-discover-what-target-you-are-in)<br>
- [**Debugging emergency mode**](#debugging-emergency-mode)

# Introduction

So.. something's wrong and you are unable to boot to your default state. Finding yourself in an odd looking shell, how do you investigate and get back to the default system?

# Boot procedure explained

Here is a simplified breakdown of the boot procedure in RHEL 8. 

| Boot order position |Procedure | Description |
|--|-- |--
|1. | BIOS | Executes Power-On-Self-Test (POST), loads `MBR`.|
|2. | MBR | Loads `GRUB2` bootloader.|
|3. | GRUB 2| Loads the `vmlinuz` kernel image, extracts the contents of the `initramfs` image.|
|4. | KERNEL | Loads necessary driver modules from the `initrd` image, starts the first process `systemd`.|
|5. | SYSTEMD | Reads configuration files from the `/etc/systemd` directory, reads files linked by `systemd` process `default.target`, brings the system to the state defined by the `default.target`.|

And a here's a picture, why not.<br><br>
![](https://static.thegeekstuff.com/wp-content/uploads/2011/02/linux-boot-process.png)

And another, why not.<br><br>
![](https://media-exp1.licdn.com/dms/image/C5622AQHWND6LtQCgfA/feedshare-shrink_1280/0/1665211087437?e=1668038400&v=beta&t=-xtocIjyBqhvfMZmKJeaH9EuHeFDIVOm74eFWg5bhgQ)

In RHEL 8, the last stages of the boot procedure could also be understood as a `systemd` `target` (which is a specific group of systemd `units` to achieve a specifc system state) or what was referred to as `SysV Runlevels`.

So, if the boot procedure successfully completes, the system will (by default) be in the `systemd` `multi-user` or `graphical` target state. If the boot procedure errors, the system will be in the `emergency` or `rescue` target state (depending on the nature of the error).

| Boot procedure | Systemd Target (a specific group of systemd `units` to achieve a specifc system state)
|-- |--
| BIOS | No target available. |
| MBR  | No target available. |
| GRUB2 | No target available. |
| KERNEL | `emergency.target`, `rescue.target` |
| SYSTEMD | `multi-user.target`, `graphical.target`|

# Requirements

| Updated | `04/2020` | 
| Linux | `Kernel 5.4` `RHEL 8 4.18` |

# Emergency.target vs. rescue.target

The differences between emergency and rescue targets are relatively simple. 

Rescue mode (also known as single-user mode) runs `mounts`, `swaps`, `udev`, `sysctl` etc.. If any of these error, the system will boot into emergency mode - the `emergency.target`.

Emergency mode is just the root mounted filesystem, read only - no other local file systems are mounted and network interfaces are not activated.

`N.B.`

You can assign the target you want to boot into at the boot menu screen in `RHEL`.<br><br>
![image-boot-menu-screen](https://user-images.githubusercontent.com/26765027/93582350-42d8b680-f99a-11ea-8c2e-b5098b85f5d1.png)

Press **`e`** to go to the Grub2 options menu at the boot menu screen. Then append `systemd.unit=rescue.target` or `systemd.unit=emergency.target` using your keyboard. (Removing `rhgb quiet` from here will also boot in verbose mode, letting you see a trace of the boot process and you can make these changes persistent in the `/etc/default/grub` file. Good to know!).<br><br>
![image-boot-options-menu](https://user-images.githubusercontent.com/26765027/93582339-3eac9900-f99a-11ea-929c-0d22632d1cfd.png)

Nice!

# How to discover what target you are in

Okay, so you've just been dropped into a shell. (If you don't have the root password, [read here](https://access.redhat.com/solutions/3889671))<br>
.![image-boot-emergency](https://user-images.githubusercontent.com/26765027/81922161-9fcef500-95d3-11ea-8344-04e1ae9cdbfa.png)

How do you find out what target you are in?

```
$ systemctl list-units --type target
```

![image-boot-find-target](https://user-images.githubusercontent.com/26765027/81923135-17e9ea80-95d5-11ea-870f-a9c86777489c.png)

<br>
Interesting, so we're in emergency mode. I wonder what services are running... this will give an idea of what stage at the boot process the failure occured.

```
$ systemctl list-units --type service --state runnning
```

![image-boot-find-running-services](https://user-images.githubusercontent.com/26765027/81923751-0f45e400-95d6-11ea-80de-380a0bbe4cdf.png)

<br>

# Debugging emergency mode

As the systemd logging service `journald` is active, we can query systemd logs we want to using `journalctl -xb`. However, as we know about targets, and that targets are a group of `systemd` `units` which are daemons integral to the running of the system, let's see if any units have failed.

```
$ systemctl --state=failed
```

![image-emergency-systemd-state-failed](https://user-images.githubusercontent.com/26765027/81922021-66968500-95d3-11ea-88e2-21e61ac50630.png)

<br>

Great. So we know that the `mount` process has failed on directory `/bar`. Let's see if we can see the raised exception from `journald`. I like to `grep` all journald logs for flexibility around the trace.

```
$ journalctl -xb | grep bar
```

![image-boot-journalctl-logs](https://user-images.githubusercontent.com/26765027/81922053-74e4a100-95d3-11ea-9dd1-94cd71a7f361.png)

`Mount` errors will be fairly common reasons as to why the boot procedure has failed and entered emergency mode. Updating the persistent mount configuration in `/etc/fstab` has fixed a mount error in this case. 

To continue to the default boot target, run command `^ d`, `systemctl default` or `exit`. To continue to a different target run `systemctl` `start` < name >`.target`

![image-boot-continue](https://user-images.githubusercontent.com/26765027/81930932-2d651180-95e1-11ea-82a2-f5eb56fe3dbf.png)

---

Thanks. This was written by Nick Otter.
