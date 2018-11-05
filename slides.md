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
* You may see some *COW*'s
* Some kernel tricks

---

## Stack Overview

* Ansible
* *LXC / LXD*
* Molecule
* *OpenZFS*

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

## LXC and LXD

Linux *System* Containers

* As fast as bare metal (2% slower).
* Ultralight hypervisor
* Well considered CLI / REST API.

notes: Crazy high density, highly scaleable

----

## Molecule

* Consistently developed roles
* supports *multiple*:
  * instances
  * operating systems
  * distributions
  * virtualization providers
  * test frameworks
  * testing scenarios.
  * target nodes (bare-metal, virtual, cloud and/or containers)

notes: Molecule leverages Ansible provider modules in a consistent way. Currently supports azure,  docker, ec2, gce, lxc, lxd, openstack, vagrant and a customizable provider (delegated) for others. Minimal learning curve


----

## OpenZFS

[OpenZFS](https://en.wikipedia.org/wiki/OpenZFS) acts as both the volume manager and the file system and has some *Magical* properties.

* Copy-on-write (*COW*) transactional object model.
* Continuous integrity checking and automatic repair.
* lots of other interesting stuff, RAID-Z, Native NFSv4 ACLs...

notes: On it's own ZFS is a very large learning curve. Hidden behind LXD, not difficult at all. Scalable to very high storage capacities. and so on.

---

## Virtualization

### Advantages

* Isolation
* Higher density
* More effective use of resources
* ...

----

## Major types of Virtualization (Hypevisors)

* Type 1 - Native ( Bare Metal )
* Type 2 - Hosted
* Type C - OS level virtualization / Containerization / Catch all...


Note: Type C is not an "official" type.
----

### Type 1

Native (Bare Metal)

```shell
Hardware <--> Hypervisor <--> Hosted OS
```

#### Examples:

* VMware ESXi
* Xen

Note: * bare metal hypervisors
* Run directly on host's hardware
* Controls the hardware
* Manage guest operating systems
* Examples of first hypervisors
  * IBM developed in the 1960s
    * native hypervisors.
      * test software SIMMON
      * CP/CMS operating system (the predecessor of IBM's z/VM)
* Modern equivalents
  * AntsleOs[5]
  * Xen
  * XCP-ng
  * Oracle VM Server for SPARC
  * Oracle VM Server for x86
  * Microsoft Hyper-V
  * Xbox One system software
  * VMware ESX/ESXi

----

### Type 2 - Hosted

```shell
Hardware <--> Host OS <--> Hypervisor <--> Hosted OS
```

#### Examples:

* KVM
* VirtualBox
* VMware Workstation Pro

Note: These hypervisors run on a conventional operating system (OS) just as other computer programs do.
* guest operating system runs as a process on the host
* Abstracts guest OS from the host OS
* Examples:
  * VMware Workstation
  * VMware Player
  * VirtualBox
  * Parallels Desktop for Mac
  * QEMU


----

### Type C

Operating-system-level virtualization, AKA. Containerization

kernel allows the existence of multiple isolated user-space instances.

 * Containers
 * Jails and chroot jails
 * AIX Workload Partitions (WPARs)
 * Virtual environments

----

### Type C

Instances look like real computers from the point of view of programs running in them.

```shell
              _ Hypervisor

            /       |
HW -- OS -<         |

            \ ____ Hosted Instance (OS / App / Service...)

```

----

#### Examples

* Docker
* LXC (Linux System Containers)    

----

## Containers, Containers, Containers

Most, but not all, containers are:

* Portable (mobility of compute)
* Fast
* Light weight
* Exchange service feature

----

### Choosing

* Popularity
* Architecture
* Storage Management
* Client Tools and Onboarding
* Image Registry
* Application Support - microservices, enterprise applications
* Vendor Support & Ecosystem

----

### Singularity

* Designed for HPC, can run on workstations.
* Image based containers
* Users cannot become root inside container (requires root outside container)
* no root daemon owned processes
* can import docker containers

----

### Singularity Users

* MCIN users
* Calcul Quebec / Calcul Canada

----

### Docker

* Designed for isolating apps / microservices from one another.
* Docker to HPC via Singularity.
* Use of layers and disabling of persistence results in lower disk IO.
* Issues using apps that expect cron, ssh, daemons, and logging.
* uses copy-on-write (CoW) for images and containers.

Note: Docker has been ported to Windows, Docker runs slower than other container systems.

----

### Docker Users

* MCIN users
* Development and test organizations.
* Many, many other organizations.

----

### LXC & LXD (container hypervisor)

* Acts like a "normal" OS environment: hostname, IP address, file systems, init.d, SSH access.
* Nearly as fast as bare metal, parallelism possilbe.
* Efficiently run one or more multi-process applications.
* Linux-native, highly stable, reliable and efficient.

----

### Who uses LXC / LXD

* IT Operators / DevOps
* MCIN
* Most Canonical Websites

----

## Choosing virtualization methods

* What are the desired properties?
* Isolation?
* ???

---


## References

### Comparisons

#### Web pages

* https://robin.io/blog/linux-containers-comparison-lxc-docker/
* https://robin.io/blog/containers-deep-dive-lxc-vs-docker-comparison/

#### Papers

* Formal Requirements for Virtualizable Third Generation Architectures-10.1.1.141.4815
* Analysis of Virtualization Technologies for High Performance Computing Environments
* Performance Evaluation of Container-based Virtualization for High Performance Computing Environments
* NIST.SP.800-125A-F - Security Recommendations for Hypervisor Deployment
* VIRTUALIZATION TECHNIQUES & TECHNOLOGIES: STATE-OF-THE-ART


#### Presentations

* Biondi1-hypervisors.pdf
* What place for the containers in the HPC world ?
* hpc-containers-singularity-advanced
* Live Migration of Linux Containers
* Containers for Science Reproducibility and Mobility-hpc-containers-singularity-advanced
* Streamlining HPC Workloads with Containers   
* Type C Hypervisors
