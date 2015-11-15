---
layout: post
title:  "How I built the snap packages for my snappy demo using snapcraft"
date:   2015-11-19 10:00:00
categories: snappy raspi snapcraft
image: hovercraft.jpg
---

Inside the lxc container, let’s switch to the “ubuntu” user, and clone my git repo:

{% highlight console %}
root@my-ubuntu:~# su -l ubuntu
ubuntu@my-ubuntu:~$ git clone https://github.com/alecu/minecraft-minimu9.git
Cloning into 'minecraft-minimu9'...
remote: Counting objects: 30, done.
remote: Total 30 (delta 0), reused 0 (delta 0), pack-reused 30
Unpacking objects: 100% (30/30), done.
Checking connectivity... done.
ubuntu@my-ubuntu:~$ cd minecraft-minimu9
ubuntu@my-ubuntu:~/minecraft-minimu9$ 
{% endhighlight %}

The repository contains a snapcraft.yaml file, that tells snapcraft what steps are needed to build it.
Let’s try that:

{% highlight console %}
ubuntu@my-ubuntu:~/minecraft-minimu9$ snapcraft
Pulling glue 
Building glue 
cp --preserve=all -R html /home/ubuntu/minecraft-minimu9/parts/glue/install/html
Staging glue 
Snapping glue 
Pulling spjs 
env GOPATH=/home/ubuntu/minecraft-minimu9/parts/spjs/build go get -t -d github.com/alecu/serial-port-json-server
Building spjs 
env GOPATH=/home/ubuntu/minecraft-minimu9/parts/spjs/build go build github.com/alecu/serial-port-json-server
env GOPATH=/home/ubuntu/minecraft-minimu9/parts/spjs/build go install github.com/alecu/serial-port-json-server
env GOPATH=/home/ubuntu/minecraft-minimu9/parts/spjs/build cp -a /home/ubuntu/minecraft-minimu9/parts/spjs/build/bin /home/ubuntu/minecraft-minimu9/parts/spjs/install
Staging spjs 
Snapping spjs 
Generated 'minecraft-minimu9_1_armhf.snap' snap
ubuntu@my-ubuntu:~/minecraft-minimu9$ 
{% endhighlight %}



This will produce a minecraft-minimu9_1_armhf.snap file that we can install in the base system, like this:

{% highlight console %}
(RaspberryPi2)ubuntu@localhost:~$ sudo snappy install --allow-unauthenticated \ ~/my-ubuntu-home/minecraft-minimu9/minecraft-minimu9_1_armhf.snap 
Installing /home/ubuntu/my-ubuntu-home/minecraft-minimu9/minecraft-minimu9_1_armhf.snap
2015/11/14 04:51:20.191703 verify.go:85: Signature check failed, but installing anyway as requested
Name              Date       Version      Developer 
ubuntu-core       2015-11-13 3            ubuntu    
lxd               2015-11-14 0.21-1       stgraber  
minecraft-minimu9 2015-11-14 IFTYLfgMVOad sideload  
webdm             2015-11-13 0.9.4        canonical 
pi2               2015-11-13 0.16         canonical 
(RaspberryPi2)ubuntu@localhost:~$ 
{% endhighlight %}

Since it’s trying to access the serial port, and everything under snappy runs confined, we can give access to the package like this:

{% highlight console %}
(RaspberryPi2)ubuntu@localhost:~$ sudo snappy hw-assign minecraft-minimu9.sideload \ /dev/ttyACM0
'minecraft-minimu9.sideload' is now allowed to access '/dev/ttyACM0'
(RaspberryPi2)ubuntu@localhost:~$ 
{% endhighlight %}

And with that we have our snap package installed, running and able to access the hardware port.

We can try our demo by pointing a browser at port 8989 of the raspbi’s IP address, eg: http://192.168.1.131:8989/ and we should see something like this:

----
This post is part 4 in a series of posts on how to get started writting IoT apps for the snappy Ubuntu Core.

[core-download]:    http://cdimage.ubuntu.com/ubuntu-snappy/15.04/stable
[raspi-write-sd]:   https://www.raspberrypi.org/documentation/installation/installing-images/
