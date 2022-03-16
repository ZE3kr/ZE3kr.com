---
title: SSD USB, Windows To Go and Mac
tags: []
id: '3776'
categories:
  - - Technology
date: 2019-02-26 16:49:52
languages:
  zh-CN: https://guozeyu.com/2019/02/ssd-usb-and-wtg/
---

Recently, I have purchased an SSD U disk and an SSD hard disk. I have Windows To Go installed on my SSD USB stick and it works fine on my Mac device. This article will share my experience.
<!-- more -->

## SSD USB Drive

### SSD VS HDD

TL;DR: Solid-state drives (SSDs) have about 10 times higher performance than mechanical hard disks (HDDs), and are significantly smaller in volume and weight than HDDs, but the price per unit capacity is also nearly 10 times higher.

Solid State Drives (SSD) have become a relatively popular concept. Generally, the performance of solid-state drives is better than that of traditional mechanical hard drives (HDDs), including but not limited to read and write speed, IOPS and other indicators. The read and write speed of an ordinary mechanical hard disk is about 100Mbyte/s, while the solid-state hard disk can reach more than 1000Mbyte/s, which is nearly 10 times faster than a mobile hard disk. Compared with HDD, the IOPS performance of SSD can be improved by nearly 100 times. These performance improvements are reflected in various aspects: copying files, opening documents and software software stored on the hard disk, starting the system, etc., up to a dozen times faster (if the bottleneck is reading and writing on the hard disk). Of course, the price of SSD can also be nearly 10 times higher than that of HDD. In addition, HDDs are much louder than SSDs, which are basically silent.

Most of the mobile hard drives sold on the market are low-speed HDDs, because ordinary consumers are most concerned about capacity and price. The price per GB of HDDs is lower, while SSDs are much more expensive, so the sales of SSDs are relatively much lower. For 300 you can get a 1TB HDD, while a 1TB SSD might cost 3000.

**Do I need an SSD? **Depends on whether there is a high performance requirement. For example, **the operating system and software are suitable to be installed on the SSD**, which will greatly reduce the boot time. Some data such as documents, pictures, and videos may not require an SSD as much. If you are an image/video worker and need to store high-quality images/videos, then SSD will be very useful, it can greatly reduce the loading time of files; of course, many software are also optimized for low-performance devices, such as Lightroom Classic CC can generate previews to reduce file size; Final Cut Pro and Premiere CC can generate proxies for original footage to reduce file size, you can store previews or proxies on SSD and original files on HDD cut costs. In addition, some infrequently accessed data can also be stored on the HDD to save costs, such as data backup, surveillance video, logs, etc.

![mSATA mini type SSD (from amazon.com)
3.0cm \* 2.7cm \* 0.4cm](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/d8ddc0db-da8b-42df-3ffd-aa2714f9e400/large)

In addition, SSD can have lighter weight and smaller volume, so that SSD can be made into the size of U disk. The HDD will be much larger. Take the smallest 2.5-inch HDD that is commonly used today as an example, its length is 10cm and the width is 7cm, not including the casing part. The length, width and height of the SSD shown in the picture above are only 3.0cm \* 2.7cm \* 0.4cm, and the weight is less than 40g. It is less than one-third the length of an HDD.

SSDs can also be very large, for example, the 15-inch MacBook Pro can even choose an SSD with a capacity of up to 4TB. But because SSDs are too expensive, large-capacity SSDs are not popular.

### SSD U-disk and Mobile HDD

SSD peripherals have such high read and write speeds that only 5Gbps USB3.0 and USB3.1 Gen1, or 10Gbps USB3.1 Gen2 can be used to reflect its performance, and the speed of these interfaces is enough for SATA SSDs . However, to achieve its full performance, a 20Gbps Thunderbolt 2 or 40Gbps Thunderbolt 3 interface is required, as most NVMe SSDs have read and write performance greater than 10Gbps but not more than 20Gbps. It's almost pointless to use a 480Mbps USB2.0 SSD, so you'll be hard-pressed to find such a product. The prices of SSD peripherals with different interfaces are also clearly differentiated. The higher the interface used, the more expensive it is. A mobile SSD with a Thunderbolt port will be 2~3 times more expensive (2~4 times faster) than a USB port.

![Interface protocol comparison chart (from thunderbolttechnology.net)](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/d37bab96-289f-464c-c6b7-a670f62a1300/large)

The shape of the Thunderbolt3 interface is exactly the same as that of the USB Type-C. Some mobile hard drives use the Type-C interface, but it does not mean that it is Thunderbolt3. You may need to see if it supports Thunderbolt by looking for the Thunderbolt ⚡️ logo. In addition, mobile hard disks using Thunderbolt3 interface may not support USB protocol. For example, the HP P800 mobile hard disk I purchased does not support USB protocol, and cannot be converted to USB, nor can it be converted to Thunderbolt2, so its compatibility is greatly reduced. . USB3.x devices are usually compatible with USB2.0, and USB Type-A and Type-C can also be transferred to each other, so USB devices have better compatibility.

**SSD mobile hard disk** is similar to ordinary mobile hard disk (based on HDD), but has higher performance. Generally, SSD mobile hard drives are also smaller in size.

**SSD U-disk** is equivalent to a smaller SSD mobile hard disk, and its volume may be similar to an ordinary U disk. Due to the reduction in size, the read/write performance and heat dissipation performance of SSD U disks are generally inferior to SSD mobile hard disks. And even fewer USB sticks that support Thunderbolt.

![From left to right: iPhone XS, HP P800 Thunderbolt3 SSD, CHIPFANCIER USB3.1 Gen2 Type-C SSD](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/1e497072-a5ef-40a1-799b-9744fc043b00/large)

| | HP P800 SSD | CHIPFANCIER SSD |
| ----- | ----- | ----- |
| Interface | Thunderbolt3 | USB3.1 Gen2 Type-C |
| Type | Mobile Hard Disk | U Disk |
| Size (mm) | 141x72x19 | 72x18x9 |
| Read Speed ​​| 2400Mbyte/s |
| About 500Mbyte/s |
| Write Speed ​​| 1200Mbyte/s | About 450Mbyte/s |
| Capacity | 256GB/512GB/1TB/2TB |
| (2TB version not released yet) | 128GB/256GB/512GB/1TB |

![On a MacBook Pro, even if a CHIPFANCIER SSD USB flash drive is installed in one port, there is still room for other Type-C devices to be connected to the port next to it. ](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/3c40f228-fe6c-4fd9-fc85-45ef9fcfff00/large)

## Disk Format Selection

Usually the hard disk needs to be formatted before use. When formatting a hard drive, you need to select the disk format. Compare four formats here: APFS, NTFS, exFAT, and FAT32. (Some software and documents can only be installed or stored in a specific disk format, so they are no longer listed in the table)

| | APFS | NTFS (v3.1) | exFAT | FAT32 |
| ----- | ----- | ----- | ----- | ----- |
| Year of release | 2017 | 2001 | 2006 | 1977 |
| Applicable media | Recommended SSD |
| Unlimited | Unlimited | Unlimited |
| Windows Compatible | – | Support | 7+**** |
| Support |
| macOS Compatible | 10.12+ |
| (Sierra) | Support* | Support | Support |
| Linux Compatible | – | Supported* | – | Supported |
| Mobile Compatible | – | – | Support | Support |
| Snapshots | Snapshots | Shadow Copy** | – | – |
| Encryption | FileVault | BitLocker** | – | – |
| Copy-on-Write | Support | – | – | – |
| Space Sharing | Support | – | – | – |
| Maximum file size*** | ∞ | ∞ | ∞ | 4GB |
| Maximum partition size*** | ∞ | ∞ | ∞ | 32GB |

* \* By default, macOS and Linux can only read NTFS disk data, not write it. Mac can read and write by adjusting system settings, and both Mac and Linux can also read and write NTFS through third-party software.
* \*\* Shadow Copy and BitLocker are generally only supported on Windows
* \*\*\* Bytes are used here instead of bits. Anything larger than 1000TB is treated as ∞. **FAT32 can have larger partition sizes (eg 2TB) in some operating systems. **
* \*\*\*\* Notwithstanding the original versions of Windows XP and WIndows Vista, Windows XP with KB955704 installed, and Windows Vista with SP1 or SP2 updates, supports exFAT to some extent.

**About operating system compatibility**, if the operating system is supported by most mainstream versions, it is counted as "supported". _Mobile Compatibility_ "Supported" as long as it is compatible with both iOS and Android.

**About iOS**, currently the iOS system only supports the import of pictures and videos from external storage, and the pictures and videos in external storage must be named according to the camera file structure; although the built-in storage of iOS uses APFS, it does not support External storage for APFS.

The disk format has little effect on the performance of the hard disk, mainly depends on its compatibility and function. But good compatibility has few functions, and many functions have poor compatibility.

## Advanced: Windows To Go

By using SSD peripherals, you can expand your computer's storage while maintaining high performance. However, you can even install your computer's operating system on a USB stick so that you can boot with the target operating system without partitioning. If you install the operating system on the HDD mobile hard disk, its booting speed will be very slow; if you install it on the ordinary U disk, the booting speed will be almost unbearable. Therefore **it is recommended to install the operating system on the SSD peripheral**.

macOS, Windows and Linux can all be installed on a USB stick or removable hard drive. But Windows is the best compatible with all kinds of hardware. If you need to use it on a PC, then only the Windows system is recommended. Windows 10 Enterprise supports the Windows To Go feature, which is optimized for installation on a USB drive or removable hard drive.

To install Windows To Go (WTG), you need a Windows environment, which can be a PC with Windows installed, a Mac using BootCamp, or a virtual machine running Windows. If you happen to be using Windows 10 Enterprise, then you can use the WTG tool in the system control panel (however, I failed to install it with the tool that comes with the system). Otherwise you can only install WTG using third-party tools. [This WTG helper tool](https://bbs.luobotou.org/thread-761-1-1.html) is recommended here.

After the installation is successful, you can choose to boot from the U disk when the computer starts, or boot from the U disk in the virtual machine. Here's how to start it on a Mac and launch in WTG and VMware Fusion on macOS.

<figure>
  <div style="position: relative; padding-top: 56.25%;"><iframe src="https://iframe.videodelivery.net/d99af30ada995905d245761ca8ea3fd9?muted=true&preload=metadata" style="border: none; position: absolute; top: 0; left: 0; height: 100%; width: 100%;" allow="accelerometer; gyroscope; autoplay; encrypted-media; picture-in-picture;" allowfullscreen="true"></iframe ></div>
  <figcaption>The MacBook Pro boots into Windows To Go. </figcaption>
</figure>

The above video is booting directly into WTG on a Mac. If your Mac is equipped with a T2 security chip, then you need to [turn off boot security checks](https://support.apple.com/en-us/HT208330) (no security, allow booting from external media) to be able to Start Windows from an external device. You might consider turning on a firmware password for continued security (Macs without a T2 chip can also turn on a firmware password). Tip: Please remember the firmware password. Once the firmware password is forgotten, it can only be returned to the factory for repair.

A freshly installed Windows may not have drivers for your Mac's mouse and touchpad. You'll need a USB mouse and touchpad to complete the setup (Magic Keyboard 2 and Magic Trackpad 2 also work as USB peripherals when wired).

You need to install the Mac driver for Windows. The Windows Support Software (drivers) can be manually "downloaded" from the Actions menu in the Mac's Boot Camp software. Note that the drivers of different models of Macs are different, it is recommended to use the system software download. If you need to use a WTG on multiple Macs, you will need to install multiple drivers. Boot Camp Assistant is to help users install Windows dual system on Mac local hard disk, **cannot** be used to install Windows To Go, here **just use it to install drivers**.

![BootCamp Download Windows Support Software Screenshot](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/022c0c81-8173-47c2-db60-c05062e10000/large)

**It is recommended to save the driver on another USB disk recognized by Windows** (see "Selecting the Disk Format" in this article). Then boot into WTG again and install it.

<figure>
  <div style="position: relative; padding-top: 56.25%;"><iframe src="https://iframe.videodelivery.net/aaf2fac6d0f2848c78c39c6e633b9af7?muted=true&preload=metadata" style="border: none; position: absolute; top: 0; left: 0; height: 100%; width: 100%;" allow="accelerometer; gyroscope; autoplay; encrypted-media; picture-in-picture;" allowfullscreen="true"></iframe ></div>
  <figcaption>macOS uses VMware Fusion to enter WTG. </figcaption>
</figure>

If you have a virtual machine, you can also boot from a USB drive in the virtual machine.

### BitLocker

If BitLocker is not enabled, all user documents on the USB flash drive are stored in plaintext, which means that as long as you have your USB flash drive, all user data can be read. If BitLocker is enabled, disk data security is guaranteed. However, Mac has no related hardware support for BitLocker, so BitLocker in WTG cannot be started by default, and the configuration file under Windows needs to be modified.

Enabling BitLocker will slightly increase the system's CPU usage and slightly reduce disk performance. In addition, the disk with BitLocker enabled is not recognized by macOS, which means that the Windows partition with BitLocker enabled cannot be read on the macOS system. It is recommended to turn it on with caution.

### User Experience

Installing Windows to a USB drive not only saves disk space on your computer, but it also has an operating system that you can take with you anywhere and can be used on Macs and PCs. I don't have BitLocker turned on. I have enabled NTFS read and write on macOS, so its Windows partition can also be used as a U disk to share data.

I have installed some common software on WTG. On other PCs, I don't necessarily need to enter the WTG system to use these software, but directly enter the Program Files folder in the WTG root directory to access these software.

I installed Windows on the SSD U disk from CHIPFANCIER, which features a USB3.1 Gen2 Type-C interface, so it can be used on a new Mac without an adapter. Then I purchased a Type-C to Type-A adapter (USB 3.0) from Greenlink, which can transfer this USB flash drive to a Type-A device.

![Greenlink Type-C to Type-A](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/795a39c9-9cd4-413d-7651-289d8c5e9000/large)

![Green Link Type-C to Type-A](https://imagedelivery.net/6T-behmofKYLsxlrK0l_MQ/9e4293ae-7568-4723-6493-501779cd3000/large)