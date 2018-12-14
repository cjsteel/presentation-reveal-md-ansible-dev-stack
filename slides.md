---
title: An Automation Development Stack
theme: night
revealOptions:
    transition: 'fade'
---

## An Automation Development Stack

One implementation of an automation and configuration stack that allows users to leverage a complex stack via a *high level of abstraction*.

Christopher Steel, MCIN ADM, December 14th, 2018

Note: I love this stack for systems admin/Dev Ops work. Users with different goals might want to modify it.

----

### Obtaining A *High Level Of Abstraction*

In this case, obtaining a *high level of abstraction*:

* Makes it "easy" to leverage the *magical* powers of some complex technology
* Allows for the creation of *provider agnostic* automation scripts which in turn
* Leads to *standardized automation roles*
* Likely to *increase velocity*

Notes: Easy to run, somewhat complicated to setup. Understsanding the entire stack is a fairly large investment but not required in order to make use of the stack.

----

### Not exactly magic but...

*"Any sufficiently advanced technology is indistinguishable from magic."*

Arthur C. Clarke's Third Law

* No rabbits, just a software stack and
* A few virtual machines and *containers* and
* Perhaps a *COW* or two?!

---

## Development Stack Overview

* *Ansible* - automation software
* *LXC / LXD* - container system and hypervisor
* Molecule - Ansible role development aid
* *OpenZFS* - Powerful file system and logical volume manager
* *VirtualBox* - Opensource Virtual Machine application

Note: FYI: Ansible requires Python and SSH. Thank You John Le and Andy Teng!

----

### Ansible

An open source automation engine

* Easy to [grok](https://en.wikipedia.org/wiki/Grok#In_computer_programmer_culture)
* Provisioning and configuration management
* Application *deployment* and intra-service *orchestration*
* Modules in abundance made using any language
* SSH / No agents required

Notes: Easy to use, rapid growth, very mature, recently added Molecule

----

### LXC and LXD

Linux *System* Containers

* As *fast* as bare metal (2% slower).
* *Ultralight* hypervisor
* Well considered CLI / REST API.

notes: Crazy high density, highly scaleable

----

### Molecule

* Allows for consistently developed Ansible roles
* Supports *multiple:*
  * instances 
  * operating systems
  * distributions
  * test frameworks
  * testing scenarios
  * virtualization providers / target nodes (bare-metal, VMs, cloud(s) and/or containers)

Notes: Brilliant move, Molecule leverages Ansible provider modules in a consistent way. Currently supports azure, docker, ec2, gce, lxc, lxd, openstack, vagrant and a customizable provider (delegated) for others. Minimal learning curve for Molecule itself, lots of sane defaults.

----

### OpenZFS

[OpenZFS](https://en.wikipedia.org/wiki/OpenZFS) acts as both a volume manager and a file system it has a few *magical* properties as well.

* Copy-on-write (*COW*) transactional object model
* Continuous integrity checking and automatic repair
* Lots of other interesting stuff, RAID-Z, Native NFSv4 ACLs...
* Ubuntu has a really nice OpenZFS, FreeNAS an appliance version.

Notes: On it's own, ZFS has quite a steep learning curve. It also happens to allow for highly scalable storage and many many other bells and whistles. ZFS gave birth to Isilon. We are going to leverage a little known feature that is not safe for production (NSFP).

----

### VirtualBox

* Open source
* Well understood
* Mature
* Runs on many OS, able to host many OS...

---

## Virtualization Overview

Before we get started with the details of this particular development stack, lets take some time and talk a bit about virtualization and how to go about choosing the right type of virtualization for your particular project or experiment.

Note: You may get to choose virtualization; in other cases, your choices may be limited.

----

### Reasons you want (need) to virtualize?

* Isolation / security
* Density / effective use of resources
* Efficiency and/or speed
* *Repeatability* and/or portability
* Required in order to...
* Other reasons

Note: Take a moment to think about the reasons that you want to virtualize. Can you think of any others?

----

### Hypervisors

What is a Hypervisor? Generally speaking a Hypervisor:

* Allows a physical *host* to operate multiple VMs or containers as *guests*
* Is *a process* that separates a system's OS and apps from the physical hardware
* Is *software*, but could be *embedded* software on a chip

Notes: Hypervisors have been around a long time in one form or another. Many advantages, including enabling more efficient usage of resources  and allowing system administrators and programmers to deploy, host, and debug code without jeopardizing the stability of the host system.

----

### Major types of Virtualization (Hypervisors)

* Type 1 - *Native* ( bare metal )
* Type 2 - *Hosted* ("on top of" OS)
* Type C - *OS level virtualization* / Containerization / Other

Note: If you Google "Virtualization Types", your results will vary widely and may all be "correct". Type C is not considered to be an "official" virtualization type by some folks. Native (No host OS), Hosted, probably an app, Type C, catch all but generally thought of as using parts of the of the host OS kernel...

----

### Type 1 - *Native* ("Bare Metal")

```shell
Hardware <--> Hypervisor <--> Hosted OS
```

* Runs directly on the hosts hardware
* Controls the hardware
* Manages the guest operating system(s)

----

####  Type 1 (Bare Metal) Hypervisor Examples:

* VMware ESXi
* Xen ( open source -  loads it's own paravirtualized host operating system)
* Xbox One system software

Note: * bare metal hypervisors
* Examples of first hypervisors
  * IBM developed in the 1960s
    * native hypervisors
      * test software SIMMON
      * CP/CMS operating system (the predecessor of IBM's z/VM)
* Modern equivalents
  * AntsleOs
  * Xen
  * XCP-ng
  * Oracle VM Server for SPARC
  * Oracle VM Server for x86
  * Microsoft Hyper-V
  * VMware ESX/ESXi

----

### Type 2 - *Hosted* Hypervisors

```shell
Hardware <--> Host OS <--> Hypervisor <--> Hosted OS
```

* Generally an application that runs on a conventional (host) operating system OS
* The guest operating system runs as a process on the host OS
* Abstracts the guest OS from the host OS

Notes:  The distinction between hypervisor types is not always clear.

----

#### Type 2 (Hosted) Hypervisor Examples:

* KVM
* VirtualBox
* VMware Workstation Pro

Note: Other examples:
  * VMware Player
  * Parallels Desktop for Mac
  * QEMU

----

#### Some properties of Type 2 Virtualization

* *Isolation* of instances
* Support for *non-linux OS*
* Well understood, *familiar* to more users

----

### Type C Virtualization

Catch all including: *containers*, *linux system containers*, *jails* and *chroot jails*, *AIX Workload Partitions* (WPARs) and *virtual environments*.

* Diverse but shared history
* *May* make use of a hypervisor
* *May* allow for the existence of multiple isolated user-space instances

Notes: While Type C is not an agreed upon official "type", people look at type C as being containers and container predecessors. You may not.

----

### Containers, Type C architectures *that employ a hypervisor*

Instances (launched containers) look like real computers from the point of view of the programs running in them.:

```shell
              _ Hypervisor*

            /       |
HW -- OS -<         |

            \ ____ Hosted Instance (OS / App / Service...)

```

Note: I'm not going to outline the architecture for any other Type C virtualization here as we would be here for quite a long time.

----

#### Examples of Type C Virtualization using Hypervisors:

* Docker - Used to be an alternative Hypervisor for using Linux System Containers (*LXC*)
* LXC (Linux System Containers) - *LXD* is an extension of the LXC hypervisor

---

## Containers


----

### Selecting the right container for the job

* *Usage* (microservice, enterprise application, development, devops, HPC?)
* *Users* (are you a developer, admin, power user or a researcher?)
* *Support* & Community, Popularity
* *Strengths & Weaknesses*
* *Sharing containers* - Image/Container Registry

Note: What are the most important features you are looking for?

----

### Singularity

*Designed for HPC*

* Can be "run" or executed directly by file name
* Image-based containers
* Must be root outside of container to become root inside of container
* No root daemon owned processes
* Can import docker containers
* Learning curve

Notes: So this information may be dated; perhaps Greg is more up to date regarding Singularity?

----

#### Singularity Users

* MCIN users
* Calcul Quebec / Calcul Canada
* HPC all over the world

----

#### Singularity Pipeline Example

After (a lot of) careful preparation...:

```shell
singularity run --app cat catdog.simg
```
Output:
```shell
 Meow , this is Cat
```

Notes: Learning curve.

----

### Docker

* Designed for *isolating applications*, *microservices*...
* Docker to *HPC* via Singularity
* Use of layers and disabling of persistence results in *lower disk IO*
* *Issues* using apps that expect cron, ssh, daemons, logging and other system stuff
* Can use copy-on-write (CoW) for images and containers

Note: Docker has been ported to Windows.

----

#### Docker Users

* MCIN
* Development and test organizations
* Many, many others

----

### LXC & LXD (Linux System Containers)

* Acts like an *OS environment*: file systems, init.d...
* Parallelism possible
* Run *one or more multi-process applications*
* Linux-native, runs well with *ZFS*.
* Windows (10 with Linux subsystem enabled) and OSX *clients*

Note: Ubuntu on Windows way uses the Linux version of the LXD client.

----

#### Who uses LXC / LXD

* IT Operators / DevOps
* MCIN
* Medical University of Gdańsk (Maciej Delmanowski) - DebOps project
* Most Canonical Websites

Note: Not well known but widely deployed for (Canonical) production sites.

----

#### LXC/LXD usage

* We cover this in the next section

----

### Choosing your virtualization target(s)

* Do you have / require root access on your virtualization project?
* What will you be virtualizing? An application, many applications, part of a pipeline?
* Host - Where will your container be running?
* Which OSs will be running on your host and guest systems?

Note: When you have a choice.

---

## Molecule

Enables the development of *provider agnostic* Ansible playbooks and roles.

* *Top* of our stack
* Built-in *provider specific Ansible tasks* (using Ansible modules)
* Creation of *custom providers*

Note: Molecule encourages the development of well-tested and portable Ansible roles that are easy to reuse.

----

### molecule init

* **molecule init role** is used to create a new *provider agnostic Ansible role* and a default scenario
* **molecule init scenario** is used to create additional scenarios for an existing Ansible role

```shell
molecule init role -r myrole
cd myrole
rm -R molecule/default
molecule init scenario -s default -d vagrant -r myrole
molecule init scenario -s lxd -d lxd -r myrole
```

Notes: Here I am deleting the default scenario generated by molecule as it is for a docker provider and we use Vagrant/VirtualBox as our default testing as all of our roles are currently tested using Vagrant/VirtualBox.

----

### Generated files

Generating a **scenario** creates a scenario directory and contents. The INSTALL.rst file includes information on any additional requirements molecule has for supporting a particular scenario:

```shell
ls -al molecule/default/
-rw-r--r-- 1 cjs cjs  260 Dec  5 20:08 INSTALL.rst
-rw-r--r-- 1 cjs cjs  303 Dec  5 20:23 molecule.yml
-rw-r--r-- 1 cjs cjs   64 Dec  5 20:08 playbook.yml
-rw-r--r-- 1 cjs cjs  235 Dec  5 20:08 prepare.yml
drwxr-xr-x 2 cjs cjs 4096 Dec  5 20:08 tests
```

Note: Docker scenarios include a default Docker file

----

### molecule.yml

* Each scenario has a **molecule.yml** file
* By default, a single instance is created called *instance*
* You may want to call it something more meaningful

```shell
nano molecule/default/molecule.yml
nano molecule/lxd/molecule.yml
```

----

### molecule create

I prefer LXC/LXD scenarios for testing as it works no matter what I am deploying and leverages the superpowers of ZFS.

```shell
# molecule check  -s lxd     # do a dry run
time molecule create         # create default instance
time molecule create -s lxd  # create lxd instance (using COW)
```

----

### Other molecule commands

Currently Molecule has a total of 16 high-level commands. Here are three:

```shell
time molecule converge -s lxd # configure the instance(s) using role.
time molecule test -s lxd     # Run all tests...
```

Notes: Again, out of the box Molecule supports azure, docker, ec2, gce, lxc, lxd, openstack, vagrant and a customizable provider.

----

### molecule list

Lists status of instances.

```shell
molecule list
```

----

### molecule destroy

Use the provisioner to destroy the instances.

```shell
molecule destroy 
```

---

## LXC/LXD

Molecule lets you control "everything" so you do not need to do this. We will take a peek under the hood to more closly examine our ZFS backed LXC/LXD installation if we have time.

----

### ZFS Storage pool

Check our space usage

```shell
lxc storage info lxd
```

----

### Launching LXC Containers With LXD

Starting a container called "c1"

```shell
lxc launch ubuntu:16.04 c1
```

Check our space usage now:

```shell
lxc storage info lxd
```

Note: We can launch a few containers. In addition to using our hosts kernel, CoW will do it's magic as well giving some unexpected results.

----

### List our containers

```shell
lxc list
```

---

## Some Key Points

* *High levels of abstraction* rock
* Multiple scenarios makes migrating to and from providers easy
* Allows us to develop *provider agnostic* Ansible roles
* *Increase in velocity*

----

![a cow](resources/imgs/cow-royalty-free-pictures-images-and-stock-photos-istock.jpg)

## References

----

### Web pages

* https://robin.io/blog/linux-containers-comparison-lxc-docker/
* https://robin.io/blog/containers-deep-dive-lxc-vs-docker-comparison/
* https://www.xenproject.org/users/virtualization.html

----

#### Papers

* Formal Requirements for Virtualizable Third Generation Architectures-10.1.1.141.4815
* Analysis of Virtualization Technologies for High Performance Computing Environments
* Performance Evaluation of Container-based Virtualization for High Performance Computing Environments
* NIST.SP.800-125A-F - Security Recommendations for Hypervisor Deployment
* VIRTUALIZATION TECHNIQUES & TECHNOLOGIES: STATE-OF-THE-ART

----

#### Presentations

##### MCIN

* http://natacha-beck.github.io/cbrain_docker/#/
* https://ibis.loris.ca/Presentations/docker.html#/

##### Other

* Biondi1-hypervisors.pdf
* What place for the containers in the HPC world ?
* hpc-containers-singularity-advanced
* Live Migration of Linux Containers
* Containers for Science Reproducibility and Mobility-hpc-containers-singularity-advanced
* Streamlining HPC Workloads with Containers   
* Type C Hypervisors

```

----
