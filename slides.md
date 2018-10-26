---
title: My Presentation
theme: night
revealOptions:
    transition: 'fade'
---

## An Interesting Ansible Development Stack

One Implementation of an Ansible Orchestration Development Stack that leverages old *magical* powers.

Note: I love this stack, other might want to consider another implementation of this highly flexible stack or may want to borrow some of the underlying ideas for other interesting projects.

---

## A Word About Magic...

*"technology sufficiently advanced is indistinguishable from magic"*

Arthur C. Clarke'a Third Law

* No rabbits, sorry.
* But we may see some *COW*'s.

----

## Stack Overview

* Ansible
* LXC / LXD
* Molecule
* *OpenZFS*

Note: Ansible requires Python and SSH

----

## Ansible

An opensource automation engine

* Easy to [grok](https://en.wikipedia.org/wiki/Grok#In_computer_programmer_culture)
* Provisioning and Configuration management
* Application deployment and Intra-service orchestration.
* Modules in abundance made using any language.
* No agent required

----

## LXC and LXD

Linux System Containers

* Run as fast as bare metal (2% slower).
* Ultralight hypervisor
* Uses kernel security features.
* Well considered CLI / REST API.

notes: Crazy high density, highly scaleable

----

## Molecule

*Results in consistently developed [Ansible](https://docs.ansible.com) roles*

Molecule provides support testing with multiple instances, operating systems and distributions, virtualization providers, test frameworks and testing scenarios. Molecule is opinionated in order to encourage an approach that results in consistently developed roles that are well-written, easily understood and maintained.

Providers can be bare-metal, virtual, cloud or containers. If Ansible can use it, Molecule can test it. Molecule simply leverages Ansible’s module system to manage instances.


notes: Molecule leverages Ansible provider modules in a consistent way. Currently supports azure,  docker, ec2, gce, lxc, lxd, openstack, vagrant and a customizable provider (delegated) for others. Minimal learning curve

----

## OpenZFS

[OpenZFS](https://en.wikipedia.org/wiki/OpenZFS) acts as both the volume manager and the file system and has some *Magical* properties.

* Copy-on-write (COW) transactional object model.
* Continuous integrity checking and automatic repair.
* lots of other interesting stuff, RAID-Z, Native NFSv4 ACLs...

notes: On it's own ZFS is a very large learning curve. Hidden behind LXD, not difficult at all. Scalable to very high storage capacities. and so on.

---

## Virtualization

Although this stack allows us to deploy to both virtual and physical (bare-metal) systems a lot of folks have mentioned that we could use some more information on virtualization and containers in particular. So I am going to squeeze a quick overview in right here and hopefully some folks who have hands on experiences specific implementations of virtualization can give us a hand with some of the nitty gritty details a little bit later on.

----

### Types of Virtualization

A long time ago, in a galaxy far far away, in the early 70's some computer person mused that one could have two basic types of virtualization, *Type 1* and *Type 2*. These days a lot of folks talk about a lot of other kinds of virtualization but we are going to stick with two, or perhaps three if required.

#### Type 1 - Hardware Virtualization - Generally Virtual Machines

* A virtual machine that acts like a real computer with an operating system.
* Software executed on these virtual machines is separated from the underlying hardware resources.

...So a Windows system can host an Ubuntu VM

Some examples might include:

* KVM
* VirtualBox
* VMware

#### Type 2 - Kernel Sharing Trickery

* Docker
* Linux Containers (LXC / LXD)
* Other "Container" like systems

#### Type 


## Type 1 Virtualization

Most people refer to *Type 1* virtualization as VM's or Virtual Machines.

Properties of Virtual Machines include the following:

Each VM has:

* It's own operating system.
* It's own software.
* Allows for the hosting of many systems on a single host.
* Can be much slower than the host system.
* Large.
* Requires a lot of resources from the host.
* Requires a VM' host application.
