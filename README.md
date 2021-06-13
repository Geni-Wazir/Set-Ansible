# Guide to setup ansible and setting up the docker and Kubernetes on the AWS ubuntu server via ansible

## Setting the servers

1. Login into your AWS account and create 2 servers with ubuntu as operating system 
2. Launch both of them and using the ssh login into the servers
3. set the password for the root user
4. Now add the ip address of both the servers in /etc/hosts file for ubuntu server an well as in the second server
   - Now the /etc/hosts file contains it's own public ip address and the public ip address of the other server 
5. follow this to enable password authentication in AWS instance
   - https://www.serverkaka.com/2018/08/enable-password-authentication-aws-ec2-instance.html

Now both the servers are ready 


# Setting ANSIBLE on local machine

In my case my local computer runs kali linux

To install ANSIBLE on kali linux 
```
sudo apt-get install ansible
```

After installing ANSIBLE we have to configure the host and ansible.cfg file which are stored in /etc/ansible directory

Add the bellow one in the ansible host file and change the 
- kubernetes_master ansible_host ip
- kubernetes_worker1 ansible_host
- ansible_password

```
[kubernetes_master_nodes]
kubernetes_master ansible_host=< ip of ubuntu >

[kubernetes_worker_nodes]
kubernetes_worker1 ansible_host=< ip of the second server >


[kubernetes:children]
kubernetes_worker_nodes
kubernetes_master_nodes

[kubernetes:vars]
ansible_ssh_user=root
ansible_password=< password_of_the_root_of_kubernetes_master ansible_host >
```

Now open the ansible.cfg file and search for the  host_key_checking

Uncoment the line by removeing the # from the line and make it False

Now our system is ready with ansible 


# Setting DOCKER and KUBERNETE on our servers

clone the file on the local system on which ansible is configured and navigate to the playbooks directory

```
git clone https://github.com/Geni-Wazir/test.git
cd test/playbooks
```

Open the env_variables file and edit the ad_addr
ad_addr refers to the master node so it should be kubernetes_master ip address

## Setting up the kubernetes master node
Now execute the followinng command to setup the kubernetes master node

```
ansible-playbook setup_master_node.yml
```

### DOCKER will be installed while running the above palybook

* Once the master node is ready, run the following command to set up the worker nodes

## Setting up the kubernetes worker node

```
ansible-playbook setup_worker_nodes.yml
```

* Once the workers have joined the cluster, run the following command to check the status of the worker nodes on kubernetes master server

```
kubectl get nodes
```

##### HERE YOU GO DOCKER AND KUBERNETE ARE INSTALLED ON AWS UBUNTU SERVER  


