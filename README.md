# Kubeadm Ansible Playbook

Build a Kubernetes cluster using Ansible with kubeadm. The goal is easily install a Kubernetes cluster on machines running:

  - Ubuntu 16.04
  - CentOS 7
  - Debian 9

System requirements:

  - Deployment environment must have Ansible `2.4.0+`
  - Master and nodes must have passwordless SSH access

# Usage

Add the system information gathered above into a file called `hosts.ini`. For example:
```
[master]
192.16.35.12

[node]
192.16.35.[10:11]

[kube-cluster:children]
master
node
```

After going through the setup, run the `site.yaml` playbook:

```sh
$ ansible-playbook site.yaml
...
 ____________
< PLAY RECAP >
 ------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||

10.0.20.11                 : ok=32   changed=16   unreachable=0    failed=0
10.0.20.12                 : ok=23   changed=13   unreachable=0    failed=0
```

Download the `admin.conf` from the master node:

```sh
$ scp k8s@k8s-master:/etc/kubernetes/admin.conf .
```

Verify cluster is fully running using kubectl:

```sh

$ export KUBECONFIG=~/admin.conf
$ kubectl get node
NAME      STATUS    ROLES     AGE       VERSION
node-11   Ready     master    4m        v1.11.0
node-12   Ready     <none>    3m        v1.11.0


$ kubectl get po -n kube-system
NAME                                    READY     STATUS    RESTARTS   AGE
etcd-master1                            1/1       Running   0          23m
...
```

# Resetting the environment

Finally, reset all kubeadm installed state using `reset-site.yaml` playbook:

```sh
$ ansible-playbook reset-site.yaml
```


# Accelerate
# - Pull images from local registry
# - Install kubernetes files from aliyun

```sh
< TASK [commons/pulling-image : Pull and tag images] >
 ----------------------------------------------------
        \   ^__^
         \  (oo)\_______
            (__)\       )\/\
                ||----w |
                ||     ||

Friday 06 July 2018  17:10:52 +0800 (0:00:00.449)       0:00:23.869 ***********
changed: [10.0.20.11] => (item=kube-controller-manager-amd64:v1.11.0)
changed: [10.0.20.11] => (item=kube-scheduler-amd64:v1.11.0)
changed: [10.0.20.11] => (item=kube-apiserver-amd64:v1.11.0)
changed: [10.0.20.11] => (item=kube-proxy-amd64:v1.11.0)
changed: [10.0.20.11] => (item=pause-amd64:3.1)
changed: [10.0.20.11] => (item=coredns:1.1.3)
changed: [10.0.20.11] => (item=etcd-amd64:3.2.18)
changed: [10.0.20.11] => (item=kubernetes-dashboard-amd64:v1.8.3)
```