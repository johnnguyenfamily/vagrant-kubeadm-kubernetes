
# Troubleshoot Guide for Kubernetes Setup

## Choose the correct combination versions of Vagrant and VirtualBox

```
Don't work: Vagrant = 2.2.15, VirtualBox = 6.1.20
Do work   : Vagrant = 2.2.15, VirtualBox = 6.1.32
```

## Vagrant up timeout

```
vagrant up
...
Timed out while waiting for the machine to boot. This means that
Vagrant was unable to communicate with the guest machine within
the configured ("config.vm.boot_timeout" value) time period.

If you look above, you should be able to see the error(s) that
Vagrant had when attempting to connect to the machine. These errors
are usually good hints as to what may be wrong.

If you're using a custom box, make sure that networking is properly
working and you're able to connect to the machine. It is a common
problem that networking isn't setup properly in these boxes.
Verify that authentication configurations are also setup properly,
as well.

If the box appears to be booting properly, you may want to increase
the timeout ("config.vm.boot_timeout") value.
```

To resolve the timeout issue, add the following to the Vagrantfile (default is 300 secs)

```sh
config.vm.boot_timeout = 1440
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

## Follow these step for each node to permanently fix above issues

Create a script

```sh
/home/vagrant/node_startup_fix.sh
```

Add the following commands to node_startup_fix.sh

```sh
#!/bin/sh
sudo swapoff -a
strace -eopenat kubectl version
```

Change node_startup_fix.sh to execute mode

```sh
chmod +x node_startup_fix.sh
```

Add commands to /etc/rc.local

```sh
sudo vi /etc/rc.local
```

with content like the following:

```sh
#!/bin/sh
# This script is executed at the end of each multiuser runlevel
/home/vagrant/node_startup_fix.sh || exit 1
exit 0
```

Change /etc/rc.local to execute mode

```sh
sudo chmod +x /etc/rc.local
```