---
layout: post
title:  "How to set up snapcraft inside a container running on Ubuntu Core"
date:   2015-11-18 10:00:00
categories: snappy raspi snapcraft
image: container-city.jpg
---

You can set up snapcraft on your PC with Ubuntu, and you’ll be able to compile and build snap packages there, but you won’t be able to run them on the snappy Ubuntu Core running on your Raspi2, because the hardware architecture is not the same. Apps that go into your snap need to be compiled for the ARM cpu in the raspi.

The best option I found for this is installing a full Ubuntu inside a container running on top of my Ubuntu Core in the raspi, and to install snapcraft there. Here’s how you can do it:

First you need to install the lxd container hypervisor, and import an Ubuntu image:

{% highlight console %}
(RaspberryPi2)ubuntu@localhost:~$ sudo snappy install lxd
Installing lxd
Starting download of lxd
22.32 MB / 22.32 MB [=========================================] 100.00 % 947.18 KB/s 
Done
Starting download of icon for package
24.52 KB / 24.52 KB [==========================================] 100.00 % 101.03 KB/s 
Done
Name        Date       Version Developer 
ubuntu-core 2015-11-13 3       ubuntu    
lxd         2015-11-14 0.21-1  stgraber  
webdm       2015-11-13 0.9.4   canonical 
pi2         2015-11-13 0.16    canonical 
(RaspberryPi2)ubuntu@localhost:~$ lxd-images import ubuntu --alias ubuntu
Downloading the GPG key for http://cloud-images.ubuntu.com
Validating the GPG signature of /tmp/tmpej4uv_fz/download.json.asc
Downloading the image.
Image manifest: http://cloud-images.ubuntu.com/server/releases/trusty/release-20151105/ubuntu-14.04-server-cloudimg-armhf.manifest
Image imported as: 2dbddcb92196b9f74dad7d7cc6e27b4267e6c96062ed1f6f9abc19e5cbdf058d
Setup alias: ubuntu
(RaspberryPi2)ubuntu@localhost:~$ lxc image list images
Generating a client certificate. This may take a minute...
If this is your first run, you will need to import images using the 'lxd-images' script.
For example: 'lxd-images import ubuntu --alias ubuntu'.
+--------+--------------+--------+------------------------------------+--------+----------+------------------------------+
| ALIAS  | FINGERPRINT  | PUBLIC |            DESCRIPTION             |  ARCH  |   SIZE   |         UPLOAD DATE          |
+--------+--------------+--------+------------------------------------+--------+----------+------------------------------+
| ubuntu | 2dbddcb92196 | no     | Ubuntu 14.04 LTS server (20151105) | armv7l | 111.25MB | Nov 14, 2015 at 1:53am (UTC) |
+--------+--------------+--------+------------------------------------+--------+----------+------------------------------+
(RaspberryPi2)ubuntu@localhost:~$ lxc launch ubuntu my-ubuntu
Creating my-ubuntu done.
Starting my-ubuntu done.
(RaspberryPi2)ubuntu@localhost:~$ 
{% endhighlight %}

With that you’ll have an old-style Ubuntu running inside the container, where you can install every deb package that exists. We now need to get a shell into the container, to be able to install snapcraft and any other thing we may need:

{% highlight console %}
(RaspberryPi2)ubuntu@localhost:~$ lxc exec my-ubuntu -- /bin/bash
root@my-ubuntu:~# 
{% endhighlight %}

We’ll try installing snapcraft from the snappy-dev/tools ppa:

{% highlight console %}
root@my-ubuntu:~# add-apt-repository ppa:snappy-dev/tools
 Official PPA for the Snappy related tools.
 More info: https://launchpad.net/~snappy-dev/+archive/ubuntu/tools
Press [ENTER] to continue or ctrl-c to cancel adding it

gpg: keyring `/tmp/tmp0c21putu/secring.gpg' created
gpg: keyring `/tmp/tmp0c21putu/pubring.gpg' created
gpg: requesting key FC42E99D from hkp server keyserver.ubuntu.com
gpg: /tmp/tmp0c21putu/trustdb.gpg: trustdb created
gpg: key FC42E99D: public key "Launchpad PPA for Snappy Developers" imported
gpg: Total number processed: 1
gpg:               imported: 1  (RSA: 1)
OK
root@my-ubuntu:~# apt-get update
Get:1 http://ppa.launchpad.net trusty InRelease [16.0 kB]
Get:4 http://ppa.launchpad.net trusty/main Translation-en [2426 B]             
[…]
Fetched 10.3 MB in 53s (191 kB/s)                                              
Reading package lists... Done
root@my-ubuntu:~# apt-get install snapcraft
Reading package lists... Done
Building dependency tree       
Reading state information... Done
[...]
The following NEW packages will be installed:
  binutils build-essential bzr cpp cpp-4.8 dpkg-dev fakeroot fontconfig-config
[...]
0 upgraded, 116 newly installed, 0 to remove and 9 not upgraded.
Need to get 48.5 MB of archives.
After this operation, 178 MB of additional disk space will be used.
Do you want to continue? [Y/n] y
Get:1 http://ports.ubuntu.com/ubuntu-ports/ trusty-updates/main libasan0 armhf 4.8.4-2ubuntu1~14.04 [56.8 kB]
[...]
{% endhighlight %}



It will take some time installing every package in the lxc inside the raspi, but when done we’ll have all the tools there to build snap packages.

I suggest an extra step, to be able to easily retrieve files built in the lxc container. So starting in the Ubuntu Core prompt, let’s create a link to the home folder inside the container root filesystem:

{% highlight console %}
(RaspberryPi2)ubuntu@localhost:~$ ln -s /var/lib/apps/lxd/current/lxd/containers/my-ubuntu/rootfs/home/ubuntu/ ~/my-ubuntu-home
{% endhighlight %}

With that we'll be able to access any file created inside the container with the shortcut `~/my-ubuntu-home` 


----
This post is part 2 in a series of posts on how to get started writting IoT apps for the snappy Ubuntu Core.

[core-download]:    http://cdimage.ubuntu.com/ubuntu-snappy/15.04/stable
[raspi-write-sd]:   https://www.raspberrypi.org/documentation/installation/installing-images/
