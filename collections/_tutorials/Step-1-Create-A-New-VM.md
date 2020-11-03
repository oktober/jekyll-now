---
layout: post
title: Step 1 - Create a New VM (Virtual Machine)
order: 2
tags: mint-laravel
description: 
date: 2020-09-26
---

**Tutorial: How to Install Laravel 7 on VirtualBox VM Running Linux Mint 19**

To install Laravel on our local machine, we need to first:

- Download and install Oracle VirtualBox
- Download a copy of Linux Mint 19
- Create a new virtual machine

Let's get started.

<!--more-->
---

## 1. Download and Install Oracle VirtualBox

[Oracle VM VirtualBox](https://www.virtualbox.org/){:target="_blank"} is free software to run virtual machines on your computer (*for personal use only*).

If you will be using the virtual machine for any kind of commercial use, you can purchase a business product like [Parallels](https://www.parallels.com/){:target="_blank"}.

Regardless of which option you choose, download the VM software you need an install it.

## 2. Download a Copy of Linux Mint (19)

Linux Mint is a lightweight version of Linux. I love it because it has a minimal, but highly usable, GUI (*Graphical User Interface*) and runs great on a virtual machine.

You can download a copy of [Linux Mint from their website](https://linuxmint.com/){:target="_blank"}.

You can choose from 3 different desktops - Cinnamon, MATE, or Xfce - any of them will work. I like to keep my machine more lightweight, since I mostly use it for programming, so I like Xfce.

## Create a New Virtual Machine

Once you've got VirtualBox installed and opened, you can create your virtual machine

{:start="3"}
3. Click on the **New** button

	![Oracle VM VirtualBox Manager with 'New' button highlighted](/assets/images/laravel-on-mint-post/create-1-select-new.png)

4. Name and operating system settings
	- **Name:** Name your virtual machine (e.g. laravel-vm)
	- **Machine Folder:** Select where you want to save the VM
	- **Type:** Change to Linux
	- **Version:** Change to either Ubuntu 64-bit or Other 64-bit
	- Click **Continue**

	![Name and operating system setup for a new virtual machine](/assets/images/laravel-on-mint-post/create-5.png)

5. Memory size settings
	- Select the amount of memory (RAM) you want
	- Click **Continue**

	*I prefer at least 1028 MB, but often choose 2048 MB since I've had a lot of memory issues with my virtual machines*

	![Memory size setup for a new virtual machine](/assets/images/laravel-on-mint-post/create-6-memory.png)

6. Hard disk settings
	- Leave the default option to 'Create a virtual hard disk now'
	- Click **Create**

	![Hard disk setup for a new virtual machine](/assets/images/laravel-on-mint-post/create-7-hd.png)

7. Hard disk file type settings
	- Leave the default option for a 'VDI (VirtualBox Disk Image)'
	- Click **Continue**

	![Hard disk file type setup for a new virtual machine](/assets/images/laravel-on-mint-post/create-8-hdtype.png)

8. Storage on physical hard disk settings
	- Select whether you want your hard drive to be dynamically allocated or fixed size
	- Click **Continue**

	*I usually choose dynamically allocated*

	![Storage on physical hard disk setup for a new virtual machine](/assets/images/laravel-on-mint-post/create-9-storage.png)

9. File location and size settings
	- Select the maximum hard drive size
	- Click **Create**
	
	*Mint 19.3 needs at least 10.7 GB just to install, but I like to give my virtual machines around 20 GB just in case*

	![File location and size setup for a new virtual machine](/assets/images/laravel-on-mint-post/create-10-location.png)

10. Once you've hit create, you should now see your new virtual machine in the Oracle VM VirtualBox Manager

	![List of virtual machines](/assets/images/laravel-on-mint-post/create-11.jpg)

11. With your new VM (virtual machine) selected, click **Settings**

	![VirtualBox Manager Tools menu with 'Settings' button highlighted](/assets/images/laravel-on-mint-post/create-12.png)

12. Next, click **Storage** at the top
	- Select the **Empty** option under 'Controller: IDE'
	- Click the round disk next to the 'Optical Drive:' drop-down list

	![Storage settings with 'Storage', 'Empty', and disk icon highlighted](/assets/images/laravel-on-mint-post/create-13.png)

13. From the menu, select **Choose a disk file**

	![Drop-down menu under 'Optical Drive' with 'Choose a disk file...' highlighted](/assets/images/laravel-on-mint-post/create-14.png)

14. Navigate to the directory where you saved the downloaded Linux Mint file
	- Select it
	- Click **Open**

	![Downloads folder open with the linuxmint iso and the Open button highlighted](/assets/images/laravel-on-mint-post/create-15.jpg)

15. Click **OK** to save your changes and close the Settings screen

	![Storage settings with 'OK' button highlighted](/assets/images/laravel-on-mint-post/create-16.png)

## Next Steps

Continue on to the next post - [Start the virtual machine and install Linux Mint, then run updates]({% link _tutorials/Step-2-Install-Mint-Run-Updates.md %})