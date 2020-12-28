# Kubernetes Networking - CNI Setup

As for it concerns Kubernetes networking, we selected [Project Calico](https://www.projectcalico.org/), since it is one of the most popular CNI plugins.
In short, it limits the overhead by requiring no overlay and supports advanced features such as the definition of network policies to isolate the traffic between different containers.

## Calico Installation
In order to install Calico, you can perform the following operations, which will download the default configuration from the official webpage and apply it customizing the pod network CIDR according to the selected cluster setup:

```bash
$ export CALICO_VERSION=v3.16
$ curl https://docs.projectcalico.org/${CALICO_VERSION}/manifests/calico.yaml -o calico.yaml
$ kubectl apply -k .
```

## SSH bastion IP pool creation
In order to distinguish ssh requests that come from outside the cluster from the ones that come from inside the cluster (i.e. using a client VM) an IP pool is used.

The sshd configuration inside VMs should have a rule restricting authentication to public key only if the source IP belongs to the IP pool, both public key and password authentication otherwise.

In order to create the IP pool, calicoctl is needed. To install it follow the instructions in the [official documentation](https://docs.projectcalico.org/archive/v3.16/getting-started/clis/calicoctl/install).
```bash
calicoctl apply -f ssh-bastion-ip-pool.yaml
# or using calicoctl as kubectl plugin
kubectl calico apply -f ssh-bastion-ip-pool.yaml 
```

## Selected cluster networking configuration
- IP addresses of pods: 172.16.0.0/16
- IP addresses of services: 10.96.0.0/12
- IP addresses of ssh-bastion pods: 172.17.0.0/26
