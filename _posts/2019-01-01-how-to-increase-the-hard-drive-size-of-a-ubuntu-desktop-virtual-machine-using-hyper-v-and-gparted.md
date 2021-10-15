---
layout: post
title:  "How to Increase the Hard Drive Size of a Ubuntu Desktop Virtual Machine Using Hyper-V and GParted"
date:  2019-01-01
categories: [technology]
images: michaeldeongreen-logo.png
tags: [Hyper-V, gparted]
---

![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/michaeldeongreen-logo.png)

By default, when you use Quick Create in Hyper-v to create a Ubuntu Desktop VM, you will only be given 12GB of hard drive space. At this time there is no way to set the size of the hard drive before Quick Create provisions the Ubuntu Desktop VM. Luckily, you can use software called [gparted](https://gparted.org/download.php) to increase the size of the virtual hard drive.  
  
**Tools Needed!**  

* gparted
* Hyper-V
* Windows 10

**Download gparted iso, which can be found [here](https://gparted.org/download.php):**  
  
**In Hyper-V, on the top menu, choose Action then Quick Create and Choose Ubuntu 18.04 LTS. Create Virtual Machine & wait for the image to download**  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-increase-the-hard-drive-size-of-a-ubuntu-desktop-virtual-machine-using-hyper-v-and-gparted-001.png)  
  
**Once the image has been downloaded, choose Edit Settings and disable Checkpoints**  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-increase-the-hard-drive-size-of-a-ubuntu-desktop-virtual-machine-using-hyper-v-and-gparted-002.png)  
  
**Go ahead and connect and start the Ubuntu Desktop installation**  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-increase-the-hard-drive-size-of-a-ubuntu-desktop-virtual-machine-using-hyper-v-and-gparted-003.png)  
  
**Once done, log into the new VM, open up a terminal and type in

```bash

    df -h
```

. You will notice that the main drive is at 44% capacity**  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-increase-the-hard-drive-size-of-a-ubuntu-desktop-virtual-machine-using-hyper-v-and-gparted-004.png)  
  
**Once the installation is complete, shutdown your new VM. In Hyper-V, select your new VM and go to SCSI Controller and add a DVD Drive**  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-increase-the-hard-drive-size-of-a-ubuntu-desktop-virtual-machine-using-hyper-v-and-gparted-005.png)  
  
**In the DVD Drive settings, point the image file to the gparted iso you downloaded**  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-increase-the-hard-drive-size-of-a-ubuntu-desktop-virtual-machine-using-hyper-v-and-gparted-006.png)  
  
**Next, go into the Firmware settings and move the DVD Drive up in the boot order section**  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-increase-the-hard-drive-size-of-a-ubuntu-desktop-virtual-machine-using-hyper-v-and-gparted-007.png)  
  
**Next, we want to add more hard drive space, so go into Hard Drive setting under SCSI Controller and press Edit**  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-increase-the-hard-drive-size-of-a-ubuntu-desktop-virtual-machine-using-hyper-v-and-gparted-008.png)  
  
**In the menu wizard, you want to choose 'expand' and add more hard drive space. I have chosen to make the drive 30GB.**  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-increase-the-hard-drive-size-of-a-ubuntu-desktop-virtual-machine-using-hyper-v-and-gparted-009.png)  
  
**Connect to the VM. You will see the gparted menu and you can just push enter on everything**  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-increase-the-hard-drive-size-of-a-ubuntu-desktop-virtual-machine-using-hyper-v-and-gparted-010.png)  
  
**When you enter gparted, you will see the below and you will want to push _Fix_**  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-increase-the-hard-drive-size-of-a-ubuntu-desktop-virtual-machine-using-hyper-v-and-gparted-011.png)  
  
**You will then be presented with disk information. Noticed the unallocated space at the bottom, which is the space we just added using Hyper-V**  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-increase-the-hard-drive-size-of-a-ubuntu-desktop-virtual-machine-using-hyper-v-and-gparted-012.png)  
  
**Next, you will want to choose your root drive (mine is _/dev/sda1_), right-click and choose _Resize_**  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-increase-the-hard-drive-size-of-a-ubuntu-desktop-virtual-machine-using-hyper-v-and-gparted-013.png)  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-increase-the-hard-drive-size-of-a-ubuntu-desktop-virtual-machine-using-hyper-v-and-gparted-014.png)  
  
**You will want to use the mouse cursor and hover over the right arrow and drag it all the way over to give drive**/dev/sda1**100% of the unallocated space and press _Resize/Move_**  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-increase-the-hard-drive-size-of-a-ubuntu-desktop-virtual-machine-using-hyper-v-and-gparted-015.png)  
  
**After this is done, press the**Apply**button located in the top menu and select**Apply**when asked are you sure you want to perform the operation**  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-increase-the-hard-drive-size-of-a-ubuntu-desktop-virtual-machine-using-hyper-v-and-gparted-016.png)  
  
**Once the space has been allocated, exit gdeparted by going to the top menu, selecting**GParted**and then Quit. Once you go to the main gparted screen, choose Exit and then _Shutdown_**  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-increase-the-hard-drive-size-of-a-ubuntu-desktop-virtual-machine-using-hyper-v-and-gparted-017.png)  
  
**Note: If you get the error in the screenshot below (happens on my laptop but not my desktop), you will want to go into the Windows Task Manager, got to Details, find the**vmwp.exe**and end the task**  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-increase-the-hard-drive-size-of-a-ubuntu-desktop-virtual-machine-using-hyper-v-and-gparted-018.png)  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-increase-the-hard-drive-size-of-a-ubuntu-desktop-virtual-machine-using-hyper-v-and-gparted-019.png)  
  
**Now, connect to your VM, go to terminal and type in

```bash
    df -h
```

Ubuntu should recognize the new space you just gave the hard drive via gparted**  
![Blogs - Michaeldeongreen](https://raw.githubusercontent.com/michaeldeongreen/michaeldeongreen.github.io/master/static/img/_posts/how-to-increase-the-hard-drive-size-of-a-ubuntu-desktop-virtual-machine-using-hyper-v-and-gparted-020.png)
