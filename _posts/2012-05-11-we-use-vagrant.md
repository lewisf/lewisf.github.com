---
layout: post
title: "We Use Vagrant"
category: 
tags: []
---
{% include JB/setup %}

Our developer turnover rate @ [We Have We Need](http://www.lewisf.com/2012/05/08/we-have-we-need-my-transition/)
is about 10 weeks. Setting up development machines is annoying, especially when the newbie is inexperienced.

As an example, the **We Have We Need** stack consists at least:
1. Django 1.3 (soon to be 1.4)
2. PostgreSQL 9.0 w/ PostGIS 1.5.2 (used to be MongoDB)
3. Fabric
4. PIL
5. nginx
and so on. A newbie would take weeks to figure out why things aren't working.

We fix this with **Vagrant**. Jon [(@jnwng)](http://www.twitter.com/#!/jnwng), who has lead the project
for two quarters prior to make setting up development environments super trivial through
[Vagrant](http://ci.vagrantup.com/). What it allows us to do is create a virtual image of
some version of Ubuntu (10.04 in our case) and either preload it with dependencies or use
[Puppet](http://vagrantup.com/docs/provisioners/puppet.html), Chef, or some other provisioner 
to make sure that the dependencies are synced across all of our development machines. All a developer 
needs to do at the most basic level now is:
- install pip
- install Fabric
- install VirtualBox
- install Vagrant
- get an image off a USB drive from one of us
- clone our repo
- and I guess make sure they have C compilers if on Windows and other crap

For example, after the basic dependencies listed above are installed, new developers can just run:

    hg clone repo
    cd repo
    vagrant init
    vagrant box add lucid32 ~/path-to-box
    vagrant up # by default provisions the box depending on defined provisions
    fab serve  # a custom Fabric command we have to bootstrap, run pending migrations, 
               # and start the dev web server

That's all it takes to have our whole stack running in a VM on Ubuntu.
[**Vagrant shares files**](http://vagrantup.com/docs/config/vm/share_folder.html) between the virtual machine and your host machine, so that
you can edit using your favorite editor on your host machine, and the changes will be
reflected in the VM. In addition, Vagrant [**port forwards**](http://vagrantup.com/docs/config/vm/forward_port.html), so I can access localhost:8000 in my browser on my host machine
to view the site in development. So development works as seamlessly as if there was no
VM there at all, and all without needing to personally figure out how to install dependencies and
getting a machine ready in less than ten minutes.

Magic.
