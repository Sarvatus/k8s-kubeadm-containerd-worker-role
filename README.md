k8s-kubeadm-containerd-role
=========

An ansible role for installing a k8s based on cri containerd via kubeadm on a Linux CentOS8.  
Support networks: weave, calico  
For now only install/setup control plane (master node)
    
    
Requirements
------------

- A compatible Linux host. The Kubernetes project provides generic instructions for Linux distributions based on Debian and Red Hat, and those distributions without a package manager.
- 2 GB or more of RAM per machine (any less will leave little room for your apps).
- 2 CPUs or more.
- Full network connectivity between all machines in the cluster (public or private network is fine).
- Unique hostname, MAC address, and product_uuid for every node.
- Certain ports are open on your machines

[Source kubernetes.io](https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)
  
  
Role Variables
--------------

#### Vars for kubeadm config file  generated to /tmp/configkube.yaml
##### For test purposes just change "ipv4_master" and "hostname" var
```
ipv4_master: 192.168.0.30  <--- ip address for your master node  
dnscluster: cluster.local  
service_subnet: 10.96.0.0/12  <--- for calico network change for 192.168.0.0/16
hostname: cent30  <--- hostname for your master node  
```

#### Additional var for disable firewalld - when < false > just creates rule for ports on control plane node: 
```
disable_firewall: false
```
#### Additional var for set network weave/calico: 
```
network: weave
```

### Vars for repository settings
```
kubernetes_yum_base_url: "https://packages.cloud.google.com/yum/repos/kubernetes-el7-$basearch"  
kubernetes_yum_gpg_key:
  - https://packages.cloud.google.com/yum/doc/yum-key.gpg  
  - https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg

docker_baseurl: https://download.docker.com/linux/centos/$releasever/$basearch/stable  
docker_gpg_key: https://download.docker.com/linux/centos/gpg
```
  
  
Example Playbook
----------------

Create play and set yours vars for control plane (master) node:
```
cat <<EOF > example-play-master-k8s.yaml
- hosts: all
  vars:
    - hostname: <master_node_hostname>
    - ipv4_master: <master_node_ip_address>
    - network: weave
    - service_subnet: 10.96.0.0/12
    - disable_firewall: false
  roles:
    - k8s-kubeadm-containerd-role
EOF
```
```
ansible-playbook -i "<master_node_ip_address>," example-play-master-k8s.yaml -k
```

License
-------

BSD

Author Information
------------------

Sarvatus 
