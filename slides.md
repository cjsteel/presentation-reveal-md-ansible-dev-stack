---
title: My Presentation
theme: night
revealOptions:
    transition: 'fade'
---
## An Ansible Development Stack

One Implementation of an Development Stack for Automation using Ansible

Note: I love this stack, other may prefer another implementation of this highly flexible stack.

----

## Why change?

* Easy to modify
* More Efficient
* Faster
* Stronger

Note: Still a steep? learning curve, but simplified by *meta* commands...

---

## Stack Overview

* Ansible
* LXC / LXD
* Molecule
* OpenZFS
* Python

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

Results in *consistently developed [Ansible](https://docs.ansible.com) roles*

* Multiple *testing frameworks*
* Multiple *testing scenarios*
* Multiple *provider "drivers"* (custom, docker, lxd, vagrant...)
* Well considered CLI

notes: Molecule leverages Ansible provider modules in a consistent way. Currently supports azure,  docker, ec2, gce, lxc, lxd, openstack, vagrant and a customizable provider (delegated) for others. Minimal learning curve

----

## OpenZFS

[OpenZFS](https://en.wikipedia.org/wiki/OpenZFS) acts as both the volume manager and the file system and has some *Magical* properties.

* Copy-on-write (COW) transactional object model.
* Continuous integrity checking and automatic repair.
* lots of other interesting stuff, RAID-Z, Native NFSv4 ACLs...

notes: On it's own ZFS is a very large learning curve. Hidden behind LXD, not difficult at all. Scalable to very high storage capacities. and so on.

----

## Python

An opensource programming / scripting language.

* Runs on and/or ships with many operating systems.
* Includes excellent support for virtual environments.

---
