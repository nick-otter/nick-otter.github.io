---
layout: post
title:  "How to setup the Lenovo T470s to dual boot"
categories: misc
---

# How to setup the Lenovo T470s to dual boot
{: style="text-align: center"}


# Contents

- [**Required**](#required)<br>
- [**Overview of steps**](#oveview-of-steps)<br>
- [**Steps**](#steps)<br>
    - [Create a hard drive partition in Windows 10](#create-a-hard-drive-partition-in-windows-10)<br>
    - [Install Ubuntu on a USB drive using third party client Rufus](#install-ubuntu-on-a-USB-drive-using-third-party-client-rufus)<br>
    - [Modify boot settings in Windows 10](#modify-boot-settings-in-windows-10)<br>
    - [Modify BIOS settings](#modify-BIOS-settings)<br>
    - [Boot to Ubuntu from the USB drive](#boot-to-Ubuntu-from-the-USB-drive)<br>
    - [Check boot order](#check-boot-order)

# Required
* Internet
* USB drive (size >= 16GB)
* Free hard drive space (size >= 30GB)
* Lenovo T470s with Windows 10.

# Overview of steps
1. Create a hard drive partition in Windows 10.<br>
2. Install Ubuntu on a USB drive using third party client Rufus.<br>
3. Modify boot settings in Windows 10.,<br>
4. Modify BIOS settings.<br>
5. Boot to Ubuntu from the USB drive.<br>
6. Install Ubuntu on the hard drive partition created in Windows 10.<br>
7. Check boot order.<br>
Done.

# Steps

## 1. Create a hard drive partition in Windows 10
Create a hard drive partition (size >= 25GB) using Disk Management ([walkthrough](https://www.youtube.com/watch?v=u5QyjHIYwTQ&feature=youtu.be&t=113)).

## 2. Install Ubuntu on a USB drive using third party client Rufus
* Download Ubuntu ISO ([walkthrough](https://ubuntu.com/download/desktop)).<br>
* Download Rufus ([walkthrough](https://rufus.ie/)).<br>
* Open Rufus.<br>
* Install Ubuntu on the USB drive and make it bootable ([walkthrough](https://ubuntu.com/tutorials/create-a-usb-stick-on-windows#3-usb-selection)). _N.B.select 'FreeDOS' in 'bootselection'. It defaults to 'non-bootable' if not chosen.

## 3. Modify boot settings in Windows 10
* Disable 'fast start up' in Windows 10 ([walkthrough](https://www.download3k.com/articles/How-to-Enable-or-Disable-Fast-Startup-in-Windows-10-01399)).

## 4. Modify BIOS settings
* Have another device with internet ready for any problems and open this link on it.<br>
* Restart and press F2 when you see the Lenovo logo to enter BIOS setup.<br>
* Modify UEFI and secure boot parameters to boot from the USB ([walkthrough](https://tothepoles.co.uk/2017/11/16/lenovo-t470p-ubuntu-16-04-install-notes/)). _N.B. select 'Both' for UEFI parameter, not 'Legacy'.<br>
* Save and exit BIOS if you have done the right thing.

## 5. Boot to Ubuntu from the USB drive
* Restart and press F12 when you see the Lenovo logo to enter boot setup.<br>
* Select the USB drive. _N.B. the USB drive will be under it's original manufacturers name unless changed._<br>
* You should now boot into Ubuntu on the USB drive.<br>
* Choose the option to test Ubuntu before installing. It might have latency problems or there may be other issues.<br>
* Install Ubuntu advanced ([walkthrough](https://itsfoss.com/install-ubuntu-1404-dual-boot-mode-windows-8-81-uefi)). Or, choose 'Install Ubuntu alongside Windows Boot Manager' - the partition you created will still be used.<br>

## 6. Check boot order
```
If after installing Ubuntu, you boot directly in Windows, check in UEFI settings for changing the boot order. If you see no option to set the boot to Ubuntu, you need to fix it from within Windows. When you are in Windows desktop, hover the mouse in left corner, right click and select administrator's command prompt. Then run the following command:

bcdedit /set "{bootmgr}" path \EFI\ubuntu\grubx64.efi

This should make the Grub (Linux boot agent) default and hence you can access both Ubuntu and Windows from it.
```

(Thanks [itsfoss](https://itsfoss.com/install-ubuntu-1404-dual-boot-mode-windows-8-81-uefi/)).

---
Thanks.
