---
title: My Presentation
theme: night
revealOptions:
    transition: 'fade'
---

## An Ansible Development Stack

One Implementation of an Ansible Orchestration Development Stack that leverages some new and old *magical* powers.

Christopher Steel, 2018

Note: I love this stack, other might want to consider another implementation of this highly flexible stack or may want to borrow some of the underlying ideas for other interesting projects.

----

### Not exactly magic but...

*"technology sufficiently advanced is indistinguishable from magic"*

Arthur C. Clarke'a Third Law

* So, no rabbits
* Virtual machines and *containers*.
* Perhaps even some *COW*'s?

---

## Development Stack Overview

* Ansible
* *LXC / LXD*
* Molecule
* *OpenZFS*
* VirtualBox (if we have time)

Note: Ansible requires Python and SSH

----

### Ansible

An opensource automation engine

* Easy to [grok](https://en.wikipedia.org/wiki/Grok#In_computer_programmer_culture)
* Provisioning and Configuration management
* Application deployment and Intra-service orchestration.
* Modules in abundance made using any language.
* No agents required

----

### LXC and LXD

Linux *System* Containers

* As fast as bare metal (2% slower).
* Ultralight hypervisor
* Well considered CLI / REST API.

notes: Crazy high density, highly scaleable

----

### Molecule

* Consistently developed roles
* supports *multiple:*
  * instances
  * operating systems
  * distributions
  * virtualization providers
  * test frameworks
  * testing scenarios.
  * target nodes (bare-metal, VM's, cloud(s) and/or containers)

notes: Molecule leverages Ansible provider modules in a consistent way. Currently supports azure,  docker, ec2, gce, lxc, lxd, openstack, vagrant and a customizable provider (delegated) for others. Minimal learning curve

----

### OpenZFS

[OpenZFS](https://en.wikipedia.org/wiki/OpenZFS) acts as both the volume manager and the file system and has some *magical* properties.

* Copy-on-write (*COW*) transactional object model.
* Continuous integrity checking and automatic repair.
* lots of other interesting stuff, RAID-Z, Native NFSv4 ACLs...

notes: On it's own ZFS is a very large learning curve. Hidden behind LXD, not difficult at all. Scalable to very high storage capacities. and so on.

----

### VirtualBox

* Opensource
* Well known
* *Universal Cake* like properties - Runs on many OS, Hosts Many OS...

---

## Virtualization Overview

Before we get started with the details of this particular development stack lets take some time and talk a bit about virtualization and how to go about choosing the right type of virtualization for your particular project or experiment because one size does not fit all when it comes to virtualization.

Note: You may get to choose virtualization, in other cases your choices may be limited.

----

### Reasons you want (need) to virtualize?

* Isolation / security
* Density / effective use of resources
* Efficiency and/or Speed
* *Repeatability* and/or portability
* Required in order to...
* Other reasons

Note: Take a moment to think about the reasons that you want to virtualize? Can you think of any others?

----

### What is a Hypervisor?

Generally speaking a Hypervisor:

* Allows a physical *host* to operate multiple VM's or containers as *guests*.
* Is *a process* that separate a systems OS and apps from the physical hardware.
* Is *software*, but could be *embedded* software on a chip.

Notes: Hypervisors have been around a long time in one form or another. Many advantages, including enabling more efficent usage of resources  and allowing system administrators and programmers to deploy, host, and debug code without jeopardizing the stability of the host system.

----

## Major types of Virtualization (Hypervisors)

* Type 1 - *Native* ( Bare Metal )
* Type 2 - *Hosted* ("on top of" OS)
* Type C - *OS level virtualization / Containerization / Other*

Note: If you Google "Virtualization Types" your results will very widely and may all be "correct". Type C is not considered to be an "official" virtualization type by some folks. Native (No host OS), Hosted, probably an app, Type C, catch all but generally though of as using parts of the of the host OS kernel...
----

### Type 1 - *Native* ("Bare Metal")

```shell
Hardware <--> Hypervisor <--> Hosted OS
```

* Runs directly on the hosts hardware
* Controls the hardware
* Manages the guest operating system(s)

----

####  Type 1 Hypervisor Examples:

* VMware ESXi
* Xen ( open source -  loads a paravirtualized host operating system)
* Xbox One system software

Note: * bare metal hypervisors
* Examples of first hypervisors
  * IBM developed in the 1960s
    * native hypervisors.
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

#### Some special properties of Type 1 Virtualization (Hypervisors)

* Speed and Flexibility?

  * Windows hosts (Xen / VMware)
  * FreeBSD (Xen)
  * NetBSD (Xen)

* Security implications

  * No host OS(!)
  * Probably a full OS on your VM's

Note: No shared kernel pieces so hypervisor can support non Linux instances.

----

### Type 2 - *Hosted* Hypervisors

```shell
Hardware <--> Host OS <--> Hypervisor <--> Hosted OS
```

* Generally an application that runs on a conventional (host) operating system OS.
* The guest operating system runs as a process on the host OS.
* Abstracts the guest OS from the host OS

Notes:  The distinction between these hypervisor types is not always clear.

----

#### Examples of Type 2 Hypervisors

* Docker (Probably belongs here now, used to be Type C.)
* KVM
* VirtualBox
* VMware Workstation Pro

Note: Familiar examples:
  * VMware Workstation
  * VMware Player
  * VirtualBox
  * Parallels Desktop for Mac
  * QEMU

----

#### Some properties of Type 2 Virtualization

* *Isolation* of instances
* Support for *non-linux OS*
* Well understood, may be *familiar* to more users.

----

### Type C Virtualzation

*linux system containers*, *jails* and *chroot jails*, * AIX Workload Partitions* (WPARs) and *virtual environments*.

* Diverse, but shared history
* *May* make use of a hypervisor
* *May* allow for the existence of multiple isolated user-space instances.
* May be *recursively virtualizable*

Notes: Again, Type C is not an agreed upon official "type". I look at type C as being Containers and containers predecessors. You may not. Type C - Containers / Catch All might help you remember this less than official category.
----

### Type C Architectures that use hypervisors

Instances look like real computers from the point of view of the programs running in them. Type C virtualization making use of a Hypervisor tends to look like this:

```shell
              _ Hypervisor*

            /       |
HW -- OS -<         |

            \ ____ Hosted Instance (OS / App / Service...)

```

A hypervisor is computer software, firmware or hardware that creates and runs virtual machines. So not all Type C virtualization takes place using a hypervisor according to this definition.

Note: I'm not going to outline the architecture for any other Type C virtualization here as we would be here for quire a long time.

----

#### Examples of Type C Virtualization using Hypervisors

* Docker - Used to be an alternative Hypervisor for using Linux System Containers (*LXC*).
* LXC (Linux System Containers) - *LXD* is an extention of the LXC hypervisor.

---

## Containers

Both Type 2 and Type C containers have some really nice features...

* Portable (mobility of compute)
* Usually Faster than Virtual machines
* Lighter than Virtual Machines
* Many others

----

### Selecting the right container

...but you will want to use the right one for the job at hand.

* *Usage* (microservice, enterprise application, development, devops, HPC?)
* *Users* (are you a developer, DevOps, power user - researcher - Systems Admin?)
* Popularity, *Support* & Community
* *Architectural Strengths & Weaknesses*
* *Image/Container Registry* / Sharing

Note: What are the most important features you are looking for?

----

### Singularity - Super Computer Containers

*Designed for HPC*

* Can be "run" or executed directly by file name.
* Image based containers
* Users cannot become root inside container (requires root outside container)
* no root daemon owned processes
* can import docker containers

Notes: So This information may be dated, perhaps Greg is more up to date regarding Singularity?
----

#### Singularity Users

* MCIN users
* Calcul Quebec / Calcul Canada

----

#### Singularity pipline example

After a careful preparation...:

```shell
singularity run --app cat catdog.simg
```
Output:
```shell
 Meow , this is Cat
```

### Docker - Application Containers

* Designed for *isolating applications* / microservices from one another.
* Docker to *HPC* via Singularity.
* Use of layers and disabling of persistence results in *lower disk IO*.
* *Issues* using apps that expect cron, ssh, daemons, and logging.
* uses copy-on-write (CoW) for images and containers.

Note: Docker has been ported to Windows, Docker runs slower than other container systems.

----

#### Docker Users

* MCIN
* Development and test organizations.
* Many, many other organizations.

----

### LXC & LXD (Linux System Containers)

* Acts like a "normal" OS environment: hostname, IP address, file systems, init.d, SSH access.
* Nearly as fast as bare metal, parallelism possible.
* Efficiently run one or more multi-process applications.
* Linux-native, highly stable, reliable and efficient.

----

#### Who uses LXC / LXD

* IT Operators / DevOps
* MCIN
* Most Canonical Websites

----

#### LXC/LXD usage

* Will will cover this in the next section

### Choosing your container(s) or virtualization method(s)

* Do you have or require root access on your virtualization project?
* What will you be virtualizing? An application, many applications, part of a pipeline?
* Where will your container be running?
* Which OS's will be running on your host and guest systems?

---

## Leveraging our Ansible Development Stack

* Molecule generates provider specific Ansible tasks using Ansible modules.
* We use **molecule** to generate *provider specific* scenarios.
* This in turn allows is to create and execute *provider agnostic* Ansible roles.

### molecule init

*molecule init* can be used to create new provider agnostic Ansible roles as well as to create provider specific scenarios. Here are two real world examples of setting up scenarios using Vagrant/VirtulaBox and LXC/LXD for xenial targets (The default target OS for Vagrant and LXD).

```shell
molecule init scenario -s default -d vagrant -r versions
molecule init scenario -s lxd -d lxd -r versions
```
### Generated files

Other files are created in the **scenario** directory. Lets take a quick peek:

```shell
ls -al molecule/default/
```

```shell
-rw-r--r-- 1 cjs cjs  260 Dec  5 20:08 INSTALL.rst
-rw-r--r-- 1 cjs cjs  303 Dec  5 20:23 molecule.yml
-rw-r--r-- 1 cjs cjs   64 Dec  5 20:08 playbook.yml
-rw-r--r-- 1 cjs cjs  235 Dec  5 20:08 prepare.yml
drwxr-xr-x 2 cjs cjs 4096 Dec  5 20:08 tests
```

### molecule.yml

Each scenario created has a corresponding `molecule.yml` file in the scenarios sub directory. The *molecule.yml* is a YAML configuration file that defines a scenario and it's instances. By default a single instance is created called *instance*. You you may want to call it something more meaningful. Lets take a look at the molecule files for scenarios we just created:

```shell
nano molecule/default/molecule.yml
nano molecule/lxd/molecule.yml
```

### Some stack *Magic* and two new molecule commands.

I prefer vagrant/virtualbox and lxd/lxd based scenarios for my testing. The LXC (Linux System Containers) make use of my host systems kernel and I use ZFS for a back end. ZFS includes *Copy On Write* (COW) so once I have a local copy of the containers base image creating addition instances is very fast and takes up almost no space. LXC containers run about 2% slower than the host OS.

```shell
time molecule check  -s lxd # use the provisioner perform a Dry-Run.
time molecule create -s lxd # create my instance using COW
```

### Other molecule commands

Molecule has a total of 16 high level commands at this time. Here are three:

```shell
time molecule converge -s lxd    # use the provisioner to configure the instance(s).
time molecule create -s lxd      # Use the provisioner to start the instance(s).
time molecule test -s lxd        # Run all tests...
```

Notes: Again, out of the box Molecule supports azure, docker, ec2, gce, lxc, lxd, openstack, vagrant and a customizable provider.

###molecule destroy

```shell
test molecule destroy 
```



## Some Key Points

* We have learned a lot and we are still learning about our new stack.
* It makes a lot of things that used to be impractical practical.
* *High levels of abstraction* rock.
* Use the right kind of Virtualization for the job at hand.
* For extensive testing you will want to leverage the fastest appropriate provider driver
* Use one or more scenarios per driver depending on your needs.

## In Closing

Raising the level of abstraction makes automation much easier as it hides many levels of complexity. In this stack this includes:

* LXC and LXD enabling the configuration and leveraging of a ZFS backend.
* molecule allowing use to ignore provider specifics once the provider and instance is configured.

This in turn means:

* Allows us to provisioning to multiple targets.
* Allows us to develop target agnostic Ansible roles.
* Enables a significant increase in development velocity.

## References

### Comparisons

#### Web pages

* https://robin.io/blog/linux-containers-comparison-lxc-docker/
* https://robin.io/blog/containers-deep-dive-lxc-vs-docker-comparison/
* https://www.xenproject.org/users/virtualization.html

#### Papers

* Formal Requirements for Virtualizable Third Generation Architectures-10.1.1.141.4815
* Analysis of Virtualization Technologies for High Performance Computing Environments
* Performance Evaluation of Container-based Virtualization for High Performance Computing Environments
* NIST.SP.800-125A-F - Security Recommendations for Hypervisor Deployment
* VIRTUALIZATION TECHNIQUES & TECHNOLOGIES: STATE-OF-THE-ART


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

```