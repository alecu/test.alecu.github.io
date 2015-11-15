---
layout: post
title:  "How to boot a Raspberry Pi 2 with the snappy Ubuntu Core"
date:   2015-11-17 10:00:00
categories: snappy raspi
image: orange-matchbox.jpg
---
Get the Ubuntu Core image from: [http://cdimage.ubuntu.com/ubuntu-snappy/15.04/stable][core-download].
You want a file named like: `ubuntu-15.04-snappy-armhf-raspi2.img.xz`

You need to write that as an image into your SD card, so you need either a computer with an SD slot, or an external SD card reader. The Raspberry Pi homepage has [very nice instructions][raspi-write-sd] on how to do it from several operating systems.

When done you can put that SD card into your Raspberry Pi 2 and start it. Make sure to plug it also to an ethernet cable that’s connected to your router.

It will take a minute or two to boot. Once booted you’ll be able to login by connecting the raspi to a tv or monitor and a keyboard. Or, you can use ssh, but you’ll first need to know the IP address. To do that, try using the “webdm.local” address, like this:

{% highlight console %}
alecu@bollo:~$ getent hosts webdm.local
192.168.1.131   webdm.local
{% endhighlight %}

If that doesn’t work, try installing the nmap package and searching your network for a host with ssh enabled:

{% highlight console %}
alecu@bollo:~$ nmap -p22 192.168.1.0/24

Starting Nmap 6.47 ( http://nmap.org ) at 2015-11-13 20:00 ART
Nmap scan report for OpenWrt.lan (192.168.1.1)
Host is up (0.00050s latency).
PORT   STATE SERVICE
22/tcp open  ssh

Nmap scan report for 192.168.1.131
Host is up (0.00044s latency).
PORT   STATE SERVICE
22/tcp open  ssh

Nmap scan report for 192.168.1.154
Host is up (0.000090s latency).
PORT   STATE SERVICE
22/tcp open  ssh

Nmap done: 256 IP addresses (3 hosts up) scanned in 3.04 seconds
{% endhighlight %}

Once you’ve found the address, you can ssh into it, using “ubuntu” as both the username and password:

{% highlight console %}
alecu@bollo:~$ ssh ubuntu@192.168.1.131
ubuntu@192.168.1.131's password: ******
Welcome to Ubuntu 15.04 (GNU/Linux 4.2.0-1014-raspi2 armv7l)

 * Documentation:  https://help.ubuntu.com/
Welcome to snappy Ubuntu Core, a transactionally updated Ubuntu.

 * See https://ubuntu.com/snappy

It's a brave new world here in snappy Ubuntu Core! This machine
does not use apt-get or deb packages. Please see 'snappy --help'
for app installation and transactional updates.

Last login: Fri Nov 13 22:37:16 2015 from 192.168.1.154
To run a command as administrator (user "root"), use "sudo <command>".
See "man sudo_root" for details.

(RaspberryPi2)ubuntu@localhost:~$
{% endhighlight %}

----
This post is part 1 in a series of posts on how to get started writting IoT apps for the snappy Ubuntu Core.

[core-download]:    http://cdimage.ubuntu.com/ubuntu-snappy/15.04/stable
[raspi-write-sd]:   https://www.raspberrypi.org/documentation/installation/installing-images/
