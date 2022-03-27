# Kubernetes-SetUp
Setting Up Kubernetes Setup in Centos OS.


**Introduction**

Kubernetes is a container orchestration system that manages containers at scale. Initially developed by Google based on its experience running containers in production, Kubernetes is open source and actively developed by a community around the world.

**Note:**

This tutorial uses version 1.14 of Kubernetes, the official supported version at the time of this article’s publication. For up-to-date information on the latest version, please see the current release notes in the official Kubernetes documentation.

Kubeadm automates the installation and configuration of Kubernetes components such as the API server, Controller Manager, and Kube DNS. It does not, however, create users or handle the installation of operating-system-level dependencies and their configuration. For these preliminary tasks, it is possible to use a configuration management tool like Ansible or SaltStack. Using these tools makes creating additional clusters or recreating existing clusters much simpler and less error-prone.

In this guide, you will set up a Kubernetes cluster from scratch using Ansible and Kubeadm.

**Prerequisites**

An SSH key pair on your local Linux/macOS/BSD machine. If you haven’t used SSH keys before, you can learn how to set them up by following this explanation of how to set up SSH keys on your local machine.

Three servers running CentOS 7 with at least 2GB RAM and 2 vCPUs each. You should be able to SSH into each server as the root user with your SSH key pair. Be sure to also add your public key to the centos user’s account on the master node. If you need guidance on adding an SSH key to a particular user account, see this tutorial on How To Set Up SSH Keys on CentOS7.

Ansible installed on your local machine. For installation instructions, follow the official Ansible installation documentation.

"https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-ansible-on-rhel-centos-or-fedora"


