---
layout: post
title: Introducing people to coding the Vagrant and Puppet way
location: San Francisco
author: paul
tags:
- teaching
- coding
- puppet
- vagrant
---

## Some Context

I'm in the early stages of contemplating whether to offer a short course introduction to coding at London Business
School (where, at the time of writing, I am just about to finish my own studies). An unscientific and horrendously
biased survey of around 10 friends suggested strong demand. Granted a similar course at Haas running through the Spring
semester is being very well received. But whilst I continue wrestle with my demand curves and logistics (booking a
lecture theatre is, it turns out, up there with particle physics), I decided to explore the feasibility from a more
technical side.

The scenario I imagined for the class is roughly:

* [Ruby on Rails](http://rubyonrails.org/) based on [OpenShift](https://openshift.redhat.com/)
* Following a guide similar to "[Getting Started with Rails](http://guides.rubyonrails.org/getting_started.html)"
* ~30 keen students working on personal laptops, the majority with no coding experience
* Class based tuition and take home exercises

Why OpenShift? Bit of an experiment to be honest, but the inspiration came from a recent presentation given by [+ryan
jarvinen](http://plus.google.com/105879391995197697236) at at the San Francisco offices of
[BranchOut](http://branchout.com/)

Given the inevitable smorgasbord of configurations that people will arrive with (granted the majority will be Macs)
having students work in a virtual machine (VM) seems the way forward. It also means people can mess around with
absolutely no fear of harming their existing setup and routine - worst comes to the worst, you download the VM and start
again (this point can be refined somewhat).

So, again, at a high level (the finer details to be worked out) this is the scenario I imagined:

* [VirtualBox](https://www.virtualbox.org/) for the virtualisation environment
* [Ubuntu](http://www.ubuntu.com/) 12.10 for the operating system
* [Gnome](http://www.gnome.org/) for the window manager - Ubuntu 12.10 uses [Unity](http://unity.ubuntu.com/) by default
but my concern here is that expecting everyone to have hardware capable of supporting 3D acceleration through the VM
is perhaps a step too far
* [RVM](https://rvm.io/) for Ruby management

Creating a VM via VirtualBox is easy. Installing Ubuntu on that VM is also easy. Setting up the development environment
is also easy. Configuring various settings is as easy as pointing, clicking and choosing a theme, background colour etc.
But all of this takes time and if for some reason I need to repeat the exercise it would be tedious at best.

## Vagrant and Puppet

[Vagrant](http://www.vagrantup.com/) is a tool to help with building development environments. It uses VirtualBox to
"build configurable, lightweight, and portable virtual machines dynamically." That is to say, Vagrant starts with some
bricks (in Vagrant terms, a base box) and helps you build a house (a VirtualBox-ready VM).

[Puppet](https://puppetlabs.com/) is a configuration management tool. To continue our rather tenuous house analogy, you
take the house that Vagrant built and use Puppet to paint the walls, install the kitchen, put furniture in place.

The best thing about both tools is that they are declarative. That means we describe the manner in which they are
constructed; with the house analogy running somewhat thin, much like an architect has plans for building a house.

Using my artistic skills, the whole process looks something like this:

<div style="text-align: center">
<img src="/images/2013-02-18-overview-of-building-vm.png"/>
</div>

<br/>

## What do I need to replicate this?

A short list of requirements (I have no experience of whether this works on Windows...anyone care to contribute?):

* [Vagrant](http://www.vagrantup.com/)
* [VirtualBox](https://www.virtualbox.org/)
* [git](http://git-scm.com/)

Then head over to [my repository on GitHub](https://github.com/myitcv/lbscoding) to follow the README which will likely
be more up-to-date than this article.

## The results?

Well for now I have Vagrant + Puppet producing a working VirtualBox VM that is student-developer-ready. The user home
directory will, in time, be sourced from GitHub which will effectively allow a student to 'reset' their development
directory to the beginning of a class (for example).

## Questions?

Please [get in touch](https://plus.google.com/u/0/105997489348527668257)
