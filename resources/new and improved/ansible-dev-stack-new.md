# A Fast and Efficient Ansible Development Stack

---

## The Problem

----

### Non homogeneous targets

* Bare Metal / Hardware
* Virtual Machines
* Containers

---

### Physical and Virtual targets

Manually preparing configuration scripts for different mediums implies:

* Lots of work to accommodate new providers and technological debt.
* New interfaces, additional learning curves and customizations.
* Difficulties in sharing as many developers are solving the same problems in different ways.

Tools like Vagrant help but requires target specific customizations as well.

---

### Containers

---

## The Stack

* Ansible
* LXD
* Molecule
* Python
* OpenZFS

----

## Ansible

An opensource automation engine

* Easy to [grok](https://en.wikipedia.org/wiki/Grok#In_computer_programmer_culture)
* Provisioning and Configuration management
* Application deployment and Intra-service orchestration, other IT needs.

----

## LXD

An opensource next generation system container manager.

* Easy to use, well considered CLI / REST API.
* As fast as bare metal (2% slower).
* Combines all available kernel security features.
notes: Very high density, highly scaleable, easy to share

----

## Molecule

Designed to aid in the development and testing of [Ansible](https://docs.ansible.com) roles.

* Well considered, easy to use (Meta?) CLI.
* Supports multiple test frameworks and testing scenarios.
* Results in consistently developed roles that are well-written, easy to understand and maintain.

notes: Leverages Ansible provider modules. Currently supports azure,  docker, ec2, gce, lxc, lxd, openstack, vagrant and a customizable provider (delegated) for others.

----

## Python

* An opensource programming language.
* Runs on and/or ships with many operating systems.
* Includes excellent support for virtual environments.

----

## OpenZFS

[OpenZFS](https://en.wikipedia.org/wiki/OpenZFS) acts as both the volume manager and the file system. 

* Copy-on-write transactional object model.
* Continuous integrity checking and automatic repair.
* lots of other interesting stuff, RAID-Z, Native NFSv4 ACLs...

notes: Scalable to very high storage capacities.

---

## install and configure LXD (and ZFS!)

* **not a suitable configuration for production**
* Assumes a "clean" system (no LXD and no ZFS).
* Currently install LXD 3.0.1 and ZFS

----

## Install zfsutils

```shell
sudo apt-get install -y zfsutils-linux
```

---

## For Ubuntu 16.04 only

* Enable Ubuntu backports by uncommenting the following in /etc/apt/sources.list:

```shell
deb http://ca.archive.ubuntu.com/ubuntu/ xenial-backports main restricted universe multiverse
deb-src http://ca.archive.ubuntu.com/ubuntu/ xenial-backports main restricted universe multiverse
```

----

## Install LXD (Ubuntu 16.04 only!)

```shell
sudo apt update
sudo apt install -y -t xenial-backports lxd lxd-client
```

Note: -t=target_release

----

## Installing LXD (Ubuntu 18.04 only!)

```shell
sudo apt update
sudo apt install -y lxd lxd-client
```

----

## You should now be a member of the lxd group

* run the `groups` command, you should see `lxd` in the output.
* log out and then log back in to ensure that your membership in the `lxd` group takes.

----

## Confirm your LXD Version

You need LXD 3.0.1 or greater for this demo

```shell
lxd --version
```

Output example:

```shell
3.0.1
```

----

## lxd init

The `lxd init` command will configure LXD for you. Go with the default answers to each questions with the following two exceptions:

* For the name of the new storage pool enter **lxd** rather than **default**
* When asked what IPv6 address to use enter **none** rather than **auto**.

Launch lxd init:

```shell
lxd init
```

----
