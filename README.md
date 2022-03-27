# Kubernetes-SetUp
**Goal :** Setting Up Kubernetes Setup in all RHEL flavour Linux OS.


**Introduction**

Kubernetes is a container orchestration system that manages containers at scale. Initially developed by Google based on its experience running containers in production, Kubernetes is open source and actively developed by a community around the world.

**Note:**

This tutorial uses version 1.23.5 of Kubernetes, the official supported version at the time of this article’s publication. For up-to-date information on the latest version, please see the current release notes in the official Kubernetes documentation.

Kubeadm automates the installation and configuration of Kubernetes components such as the API server, Controller Manager, and Kube DNS. It does not, however, create users or handle the installation of operating-system-level dependencies and their configuration. For these preliminary tasks, it is possible to use a configuration management tool like Ansible or SaltStack. Using these tools makes creating additional clusters or recreating existing clusters much simpler and less error-prone.

In this guide, you will set up a Kubernetes cluster from scratch using Ansible and Kubeadm.

**Prerequisites**
1 ) Machine with Ansible and Git installed init.

 
If Ansible is not installed on your local machine. For installation instructions, follow the official Ansible installation documentation.

"https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-ansible-on-rhel-centos-or-fedora".

If git is not installed, follow the below GIT installation Documentation.

https://git-scm.com/downloads


2 ) An SSH key pair on your local Linux/macOS/BSD machine through which ansible can login into target machines. If you haven’t used SSH keys before, you can learn how to set them up by following this
"https://www.digitalocean.com/community/tutorials/ssh-essentials-working-with-ssh-servers-clients-and-keys#generating-and-working-with-ssh-keys".

3 ) Servers running RHEL Linux flavours with at least 2GB RAM and 2 vCPUs each. You should be able to SSH into each server as the root user with your SSH key pair. Be sure that you have ssh-key to master. If you need guidance on adding an SSH key to a particular user account, see this tutorial 
"https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-centos7".




Once after all the prerequisites are okay. You can follow the below steps to create your own K8's cluster over Centos Machine.

# **Step 1**

Login into the machine where ansible and git are installed and navigate to /tmp directory.

**cd /tmp**

# **Step 2**

Clone the repository into your Machine Where Ansible & Git are Installed.

**git clone https://github.com/dasari97/Kubernetes-SetUp.git**

**cd Kuberenetes-SetUp**

# **Step 3**
Update the Machine IP details in the **hosts** file in **root** directory this repostory. I have taken only one Master machine and 2 worker machines. You can choose as per your requirement. Better to provide the private IP's. If elastic IP are available those can also be used. 

I am using VIM editor. You can choose your fav editor.

**vim hosts**


[masters]

master          ansible_host=master-ip-address          ansible_user=root

[workers]

worker1         ansible_host=worker-ip-address_1          ansible_user=root

worker2         ansible_host=worker-ip-address_2          ansible_user=root 

After entering into the IP's save and quit the hosts file (Press ESC and :wq! in VI/VIM editor to save and quit).

**Execute** the below **step 4** , **step 5** and **step 6** command in **/tmp/Kubernetes-SetUp/** directory.

# **Step 4**

**ansible-playbook -i hosts files/kube-dependencies.yml**

**Expected Output**

PLAY RECAP ****

master                     : ok=9    changed=5    unreachable=0    failed=0   

worker1                    : ok=7    changed=5    unreachable=0    failed=0  

worker2                    : ok=7    changed=5    unreachable=0    failed=0 

# **Step 5**

**ansible-playbook -i hosts files/master.yml**

**Expected Output :** 

PLAY          [master]          ****

TASK          [Gathering Facts]          ****

ok: [master]

TASK          [initialize the cluster]          ****

changed: [master]

TASK          [create .kube directory]          ****

changed: [master]

TASK          [copy admin.conf to user's kube config]          *****

changed: [master]

TASK          [install Pod network]          *****

changed: [master]

PLAY          RECAP          ****

master                     : ok=5              changed=4              unreachable=0              failed=0

If the Output is as shown above,

execute the below commands one after other:

1) **ssh root@master-ip-address** 

2) **kubectl get nodes**




**Expected Output: **

Output

NAME                STATUS              ROLES               AGE                 VERSION

master              Ready               master              1d                  v1.14.0



If everthing is done perfectly, You will be able to see the master machine ready. If not Please verify the above the steps.

Make sure the SSH connection is happening from Ansible machine into the master and worker nodes.

If output is as expected, you can exit from the master machine using :

**exit**


# **Step 6**

**ansible-playbook -i hosts files/workers.yml**



**Expected Output: **

PLAY          [master]          ****

TASK          [get join command]          ****

changed: [master]

TASK          [set join command]          *****

ok: [master]

PLAY          [workers]          *****

TASK          [Gathering Facts]          *****

ok: [worker1]

ok: [worker2]

TASK          [join cluster]          *****

changed: [worker1]

changed: [worker2]

PLAY          RECAP          *****

master                     : ok=2              changed=1              unreachable=0              failed=0   

worker1                    : ok=2              changed=1              unreachable=0              failed=0  

worker2                    : ok=2              changed=1              unreachable=0              failed=0


If the Output is as shown above,


execute the below commands :

**ssh root@master-ip-address** 

**kubectl get nodes**



**Expected Output:**

Output

NAME                STATUS              ROLES               AGE                 VERSION

master              Ready               master              1d                  v1.14.0

worker1             Ready               <none>              1d                  v1.14.0

worker2             Ready               <none>              1d                  v1.14.0
  


If everthing is done perfectly, You will be able to see the master and worker node machines ready. If not Please verify the above the steps.

Make sure the SSH connection is happening from Ansible machine into the master and worker nodes.

If output is as expected, Then your K8's cluster is all set for you.
  
**ssh centos@master_ip** to operate the cluster.

**Thanks.**
**Dasari.**
