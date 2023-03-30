---
layout: post
title: Debugging Linux Boot Performance
description: 
tags: linux cmd
---

It's been around 8 months since I made the switch to Linux (Mint) and I have to admit, this is way too good for my potato laptop XD . Opening 30+ tabs in my browser no longer causes my cpu to torment me with its scream. However, while everything seemed to be all well and good, the system boot time seemed to have gone up. Hmm, let's see what caused this degradation.

<hr>

Opening the terminal and typing <b>sudo systemd-analyze</b> gave me a rough overview of the current boot timings.

<div class="img_parent">
<img src="{{ "assets/images/2023-03-12-Debugging-Linux-Boot-Performance/systemd-analyze.png" | relative_url }}">
</div>

That's almost 1.5 mins to boot the system. Not bad (it took 3-4 mins back when I was using Windows + don't mention about updates), but can we make it faster?

Let's see what's taking up time in the userspace. For this, just pass the <b>blame</b> command to `systemd-analyze` to print list of running units ordered by time to initialize.

<div class="img_parent"><b>sudo systemd-analayze blame</b><br><br></div>

<div class="img_parent">
<img src="{{ "assets/images/2023-03-12-Debugging-Linux-Boot-Performance/blame.png" | relative_url }}">
</div>

The mysql.service takes up a whopping 31.2s to initialize, but do I need it? Actually not, as this was installed on my system as part of the DBMS course last semester and I really don't require it running on my device. Hence that entire thing goes straight to the bin. 

The next service that caught my eye was the vmware-USBArbitrator. This service is used by vmware workstation and allows USB devices plugged into the host system to be usable by the guest OS. However, as I dont use the workstation frequently nor do I require USB functionality at those times when I do use it - these services can be disabled.

<br>

<div class="img_parent">
<img src="{{ "assets/images/2023-03-12-Debugging-Linux-Boot-Performance/vmware_disable.png" | relative_url }}">
</div>

<br>

Now, this should take care of the special cases in my system's boot process. What about the other generic system services. Let's take a peek into those. 

ModemManager.service is a DBus-activated daemon that controls mobile broadband interfaces. If you don’t have a mobile broadband interface (built-in, paired with a mobile phone via Bluetooth, or USB dongle), you don’t need this. It's generally safe to disable D-Bus related services as these are restarted whenever it's required.

<div class="img_parent"><b>sudo systemctl stop ModemManager.service </b><br><b>sudo systemctl disable ModemManager.service</b><br><br></div>

The systemd-journal-flush.service keeps track of the systemd logs which can be useful in debugging potential issues with the system. Hence, instead of disabling it outright (not recommended) we can optimize it according to our use-case. For more info visit <a href="https://askubuntu.com/questions/1094389/what-is-the-use-of-systemd-journal-flush-service">this askubuntu.com post</a>.



<hr>

References: 

- <a href="http://tuxdiary.com/2012/11/06/disable-vmware-services-at-boot/">http://tuxdiary.com/2012/11/06/disable-vmware-services-at-boot/</a>

- <a href="https://www.linux.com/topic/desktop/cleaning-your-linux-startup-process/">https://www.linux.com/topic/desktop/cleaning-your-linux-startup-process/</a>

- <a href="https://askubuntu.com/questions/1094389/what-is-the-use-of-systemd-journal-flush-service">https://askubuntu.com/questions/1094389/what-is-the-use-of-systemd-journal-flush-service</a>

- <a href="https://www.freedesktop.org/software/systemd/man/journald.conf.html">https://www.freedesktop.org/software/systemd/man/journald.conf.html</a>
