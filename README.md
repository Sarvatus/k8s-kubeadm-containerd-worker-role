k8s-kubeadm-containerd-worker-role
=========

An ansible role for installing required stuff for worker node k8s based on cri containerd via kubeadm join option on a Linux CentOS8.  
Support networks: weave, calico  
Setup worker node
    
    
Requirements
------------

- A compatible Linux host. The Kubernetes project provides generic instructions for Linux distributions based on Debian and Red Hat, and those distributions without a package manager.
- 1 GB or more of RAM per machine (any less will leave little room for your apps).
- 1 CPUs or more.
- Full network connectivity between all machines in the cluster (public or private network is fine).
- Unique hostname, MAC address, and product_uuid for every node.
- Certain ports are open on your machines

  
  
Role Variables
--------------

#### Vars for  /etc/hosts

```
ipv4_node: 192.168.0.30  <--- ip address for your worker node  
hostname: cent30  <--- hostname for your worker node  
```
#### Login to master node and get join command:   kubeadm token create --print-join-command
```
kubeadm_join_command: kubeadm join 192.168.0.30:6443 --token lh4yqu.yn3q0suapys1bolk --discovery-token-ca-cert-hash sha256:05fe4ea8bf3d9b548974f4e0dff4de6b48e1b94a331d14b4f894b4d31e7428ca
```
#### Additional var for disable firewalld - when < false > just creates rule for ports on control plane node: 
```
disable_firewall: false
```
#### Additional var for set network weave/calico required for port opening: 
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

Create play and set yours vars for worker node:
```
cat <<EOF > example-play-worker-k8s.yaml
- hosts: all
  vars:
    - hostname: <worker_node_hostname>
    - ipv4_node: <worker_node_ip_address>
    - network: weave
    - kubeadm_join_command: <your_join_command_from_master>
    - disable_firewall: false
  roles:
    - k8s-kubeadm-containerd-worker-role
EOF
```
```
ansible-playbook -i "<worker_node_ip_address>," example-play-worker-k8s.yaml -k
```

License
-------

BSD

Author Information
------------------

Sarvatus 
