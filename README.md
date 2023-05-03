# DebOps
This repository provides an example configuration for DebOps, a set of Ansible roles and playbooks for building and managing infrastructure.

The configuration is intended to serve as a starting point for building infrastructure with DebOps, and includes a set of example roles and
playbooks for common tasks such as setting up a networking, grub configuration, configuring SSH, managing users, apt configuration etc.

## Requirements
To use this configuration, you will need the following:

	* A Linux system with Ansible installed
	* Access to a target infrastructure environment (e.g. cloud server, virtual machine)

## Usage
To use this configuration, follow these steps:

Clone this repository to your local machine:

```git clone https://github.com/beskar-cloud/debops-lab.git```
Change into the debops-lab directory:
```cd debops-lab```
Modify the inventory file to match your target infrastructure environment and create `hosts` file containing the inventory of hosts that are 
managed by the DebOps

### sample inventory file:
```
  # This is an Ansible inventory file in INI format. You can define a list of
  # hosts and groups to be managed by this particular inventory.

  # Hosts listed under [debops_all_hosts] will have common DebOps plays
  # ran against them. It will include services such as iptables, DNS, Postfix,
  # sshd configuration and more.
  #
  # View the list here:
  # https://github.com/debops/debops/blob/master/ansible/playbooks/common.yml
  #
  # You should check Getting Started guide for useful suggestions:
  # https://docs.debops.org/en/master/introduction/getting-started.html

  # Your host was not detected as compatible with DebOps playbooks, so you will
  # not be able to leverage the above features on your current operating system.
  # You can however use a virtual machine as the Ansible Controller.

  [debops_all_hosts:vars]
  ansible_ssh_user=ubuntu  
  ansible_python_interpreter=/usr/bin/python3 
  [all:vars]
  mgmt_gateway="10.3.0.1"

  [all]
  #computes
  cmp01 ansible_host=10.3.0.101 tunnel_address=10.3.8.101/22 ceph_public_address=10.3.16.101/22

  #ceph
  ceph01 ansible_host=10.3.1.201 ceph_internal_address=10.3.12.201/22 ceph_public_address=10.3.16.201/22 


  #control
  ctl01 ansible_host=10.3.4.11 ceph_internal_address=10.3.12.11/22 tunnel_address=10.3.8.11/22 ceph_public_address=10.3.16.11/22 octavia_address=10.3.28.11/22 


  #k8s-vms
  master01 ansible_host=10.3.4.221


  #proxy-vms
  proxy01 ansible_host=10.3.4.231 ceph_public_address=10.3.16.231/22  
  proxy02 ansible_host=10.3.4.232 ceph_public_address=10.3.16.232/22

  [computes]
  cmp01

  [controls]
  ctl01


  [ceph]
  ceph01

  [k8s-vms]
  master01.cloudpoint.tcpro.cz


  [proxy-vms]
  proxy01
  proxy02

  [debops_all_hosts:children]
  computes
  controls
  ceph
  k8s-vms
  proxy-vms

  [debops_service_kmod:children]
  computes
  controls
  ceph
  k8s-vms
  proxy-vms

  [debops_service_sysctl:children]
  computes
  controls
  ceph
  k8s-vms
  proxy-vms

  [debops_service_sysfs:children]
  computes
  controls
  ceph
  k8s-vms
  proxy-vms

  [debops_service_swapfile:children]
  computes
  controls
  ceph
  k8s-vms
  proxy-vms

  [debops_service_grub:children]
  computes
  controls
  ceph
  k8s-vms
  proxy-vms

  # Network configuration
  [debops_service_netbase:children]
  computes
  controls
  ceph
  k8s-vms
  proxy-vms

  [debops_service_ifupdown:children]
  computes
  controls
  ceph
  k8s-vms
  proxy-vms


  [debops_service_libvirt:children]
  controls

  [debops_service_libvirtd:children]
  controls

  [debops_service_libvirtd_qemu:children]
  controls

  # LVM configuration
  #[debops_service_lvm:children]
  #ceph

  [debops_service_debops_legacy:children]
  computes
  controls
  ceph
  k8s-vms
  proxy-vms
```
Modify the groups in group_vars/ folder to configure the various settings used by the roles.

## Groups

Feel free to modify groups according to the needs of infrastructure.

1.  all:
The group_vars/all/ file in the debops-lab repository contains variables and playbooks that are used across all hosts in the infrastructure. 
These variables include settings such as the timezone, domain name, and DNS server configuration, ssh configuration, user configuration etc.

2.  ceph:
The group_vars/ceph directory in the debops-lab repository contains variables that are specific to the `ceph` group of hosts. 
This group is defined in the inventory file and is used to configure ceph nodes in the infrastructure.

3.  computes:
The group_vars/computes directory in the debops-lab repository contains variables that are specific to the `computes` group of hosts. 
This group is defined in the inventory file and is used to configure compute nodes in the infrastructure.

4.  controls:
The `controls` directory in the inventory/group_vars directory of the debops-lab repository contains variables that are used to configure 
the control nodes.

5. k8s-vms and proxy-vms: 

These directory or groups in invnentory are used to configure virtual machiens. 

Run the DebOps playbooks to configure the infrastructure. For example, to configure a network configuration of compute server:

```sudo debops --private-key=<ssh-key> -l  computes service/ifupdown  -t role::ifupdown```



