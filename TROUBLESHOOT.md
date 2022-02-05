
# Troubleshoot Guide for Kubernetes Setup

## Choose the correct combination versions of Vagrant and VirtualBox

```
Don't work: Vagrant = 2.2.15, VirtualBox = 6.1.20
Do work   : Vagrant = 2.2.15, VirtualBox = 6.1.32
```

## Connection to server was refused

Issue:
```sh
kubectl get nodes

The connection to the server 10.0.0.10:6443 was refused - did you specify the right host or port?
```

Solution:
```sh
sudo swapoff -a
strace -eopenat kubectl version
```

## Worker nodes are in NotReady status

Issue:
```sh
kubectl get nodes

NAME            STATUS     ROLES                  AGE     VERSION
master-node     Ready      control-plane,master   7h26m   v1.23.3
worker-node01   NotReady   worker                 7h8m    v1.23.3
worker-node02   NotReady   worker                 6h45m   v1.23.3
```

Solution:
```sh
sudo swapoff -a
strace -eopenat kubectl version
```