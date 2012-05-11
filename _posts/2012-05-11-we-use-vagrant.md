---
layout: post
title: "We Use Vagrant"
category: 
tags: []
---
{% include JB/setup %}

Having [We Have We Need](http://www.lewisf.com/2012/05/08/we-have-we-need-my-transition/)
as a part of a university program is both a blessing and a curse. The obvious blessing is that
the students get the support of faculty, and that there is a neverending supply of students
to take on the project. The curse is that developer turn over is on average 10 weeks
(1 quarter). Sure, some developers stay a couple of quarters, but there are new developers
coming in every quarter, and it always ends up being a pain getting someone's development
machine setup with all the necessary dependencies.

As an example, the **We Have We Need** stack consists at least:
1. Django 1.3 (soon to be 1.4)
2. PostgreSQL 9.0 w/ PostGIS 1.5.2 (used to be MongoDB)
3. Fabric
4. PIL
5. nginx
and so on.

For experienced developers, this may be an easy setup, but for the inexperienced students
coming in, it becomes difficult figuring out why installs go haywire, or why PostgreSQL 
is complaining about no role or no database found (for a lot of undergraduates, this is
their first exposure to a web framework/database).

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

    hg/git clone repo
    cd repo
    vagrant init
    vagrant box add lucid32 ~/path-to-box
    vagrant up # by default provisions the box depending on defined provisions
    fab serve  # a custom command we have to bootstrap and run the dev web server

And they will have our whole stack running in a VM on Ubuntu, if everything goes properly.
**Vagrant shares the repo** between the virtual machine and your host machine, so that
you can edit using your favorite editor on your host machine, and the changes will be
reflected in the VM. In addition, Vagrant port forwards, so I can access localhost:8000 in my browser on my host machine
to view the site in development. So development works as seamlessly as if there was no
VM there at all, and all without needing to personally figure out how to install dependencies and
getting a machine ready in less than ten minutes.

Magic.
