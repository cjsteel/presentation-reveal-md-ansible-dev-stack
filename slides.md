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

Molecule provides support testing with multiple instances, operating systems, distributions, virtualization providers, test frameworks and testing scenarios. Molecule is opinionated in order to encourage an approach that results in consistently developed roles that are well-written, easily understood and maintained.

Providers can be bare-metal, virtual, cloud or containers. *Molecule simply leverages Ansible’s module system to manage instances.*

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

### Some Advantages

* high density
* Better use of resources
* Lots of choices

----

## Major types of Virtualization

* Type 1 - Native (Bare Metal)
* Type 2 - Hosted
* Type C - Container

----

## Type 1

Native (Bare Metal)

```shell
Hardware <--> Hypervisor <--> Hosted OS
```

#### Examples:

* VMware
* Xen

----

#### Type 2 - Hosted

```shell
Hardware <--> Host OS <--> Hypervisor <--> hosted OS
```

#### Examples:

* KVM
* VirtualBox

----

#### Type C - Shared host kernel

Operating-system-level virtualization, also known as containerization, refers to an operating system feature in which the kernel allows the existence of multiple isolated user-space instances. Such instances, called containers,[1] partitions, virtual environments (VEs) or jails (FreeBSD jail or chroot jail), may look like real computers from the point of view of programs running in them. A computer program running on an ordinary operating system can see all resources (connected devices, files and folders, 


```shell
             _ Hypervisor

            /      |
HW -- OS -<        |

            \ ____ Hosted OS/App/ Service

```

#### Examples

* Linux System Containers LXC.
* Docker
