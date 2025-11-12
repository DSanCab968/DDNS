# Practice 3 -- Dynamic DNS

The objective of this practice is to create a dynamic DNS using a three virtual machines: DHCP server, DNS server and a client.

![Esquema proyecto](Esquema.png)

------------------------------------------------------------------------

**Author:**\
Daniel Sánchez Cabello

**Course/Subject:** 2º ASIR B -- Network Services and Internet\
**Finish date:** 

------------------------------------------------------------------------

## Index

1.  [Vagrantfile Creation and VM's Network Configuration](#1-vagrantfile-creation-and-vms-network-configuration)
    1.  [DHCP Server](#11-dhcp-server)
    2.  [DNS Server](#12-client-1-creation)
    3.  [Client 2 Creation](#13-client-2-creation)
    4.  [Vagrantfile final result](#14-vagrantfile-final-result)

2.  [Ansible files configuration](#2-ansible-files-configuration)
    1.  [Creating Ansible configuration file](#21-creating-ansible-configuration-file)
    2.  [Creating the inventory](#22-creating-the-inventory)
    3.  [Creating the playbook](#23-creating-the-playbook)

3.  [Verification](#3-verification)
    1.  [Deploy of the playbook](#31-deploy-of-the-playbook)
    2.  [Verifying addresses](#32-verifying-addresses)



------------------------------------------------------------------------

## 1. Vagrantfile Creation and VM's Network Configuration

### 1.1 DHCP Server Creation

The first step is to tell Vagrant the box that we will use, in this case
`debian/bullseye64`.

Then we create the first VM called **server** and configure two network
interfaces:

-   **Private network** -- `192.168.56.10` → to get a host-only
    communication with our real computer.\
-   **Internal network** -- `192.168.57.10` → to communicate with the
    two clients that will be isolated from our computer.

After that, we must add the provision path. In that provision we will
set further configurations.

------------------------------------------------------------------------

### 1.2 Client 1 Creation

Now, let's create the first client called **c1**.\
This one will receive the IP from the server given a range of IPs
(`192.168.57.25 – 192.168.57.50`).\
That means that inside that range, it can receive any IP that is free
from the server.

We must also configure the network as an internal network and specify
that it will receive the IP through DHCP.\
Also, set the path for the provision file.

------------------------------------------------------------------------

### 1.3 Client 2 Creation

For the other client (**c2**), the process will be the same except for a
detail:\
the IP address it receives will be **fixed by MAC**, which means that we
will indicate its MAC address and the server will always give the same
IP to that MAC.

Add the path for the provision too (it is the same for c1 and c2).

------------------------------------------------------------------------

### 1.4 Vagrantfile final result

Now, lets create the whole vagrantfile. Wecan add the provision with ansible to the vagrantfile directly so it will be used when we set up th machines with `vagrant up` or we can provision the machines later (when they are running) using the command `ansible-playbook [-i inventoryname.yml] [playbookname.yml]`. If we already have the default playbook in the configuration file, it is not necessary to write it in the previous command, do this instead: `ansible-playbook [playbookname.yml]`

<details><summary>Vagrantfile with provision</summary>

```ruby



```
</details>

## 2. Ansible files configuration

This time we will do the provision using Ansible wich is a tool that allow us to provision in a more sofisticated and professional way. Instead of using scripts, we can make an inventory of machines adn run a playbook with tasks against the items of that inventory.

------------------------------------------------------------------------

### 2.1 Creating Ansible configuration file 

Here we must include the general configuration for Ansible. For example we can set the path for the inventory that will be used for all the playbooks, so the we do not need to specificate. If we had different inventories, then we should not include any of the here. Also we can set the remote user and other parameters.

File: `ansible.cfg`

``` ini


```

------------------------------------------------------------------------

### 2.2 Creating the inventory

The inventory is used to have an organized list of the machines we will have and their general connection details, such as the IP, port, or key. We could have used Vagrant’s generic key for all the machines and included it under the vars section, where general parameters are set, but in this case, I assigned each machine the path to the key stored in its own files.

File: `inventory.yml`

``` yml

```

------------------------------------------------------------------------

### 2.3 Creating the playbook

First, connect via SSH to the three VMs and use `ip a` to chech the network adapter used for communication and the use the command `vagrant ssh-config` para ver los puertos de cada máquina. Esto es muy importante a la hora de crear el playbook.

We will do a series of tasks to install the dhcp tools, set the subnets configuration and check that everything is alright. The first group of tasks will be run on the server and the second in the clients. Here in the playbook, we run the tasks aiming the groups created on the inventory.

File: `playbook.yml`

```yml



```

------------------------------------------------------------------------

## 3. Verification

### 3.1 Deploy of the playbook

Note: when running the playbook against machines running, there may be a error related to the time, if that occurs, reload and provision again the mchine.

<details><summary>Console output of the playbook after run</summary>

```bash


```
</details>

### 3.2 Verifiying addresses

<details><summary>Server addresses</summary>

```bash



```




------------------------------------------------------------------------

© Daniel Sánchez Cabello
