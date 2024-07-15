# Ansible-k8s-cluster
This project's main purpose is to practice ansible and also automate the process of installing kubernetes using kubeadm on an ubuntu server.

This follows the official kubernetes documentation on creating a cluster using kubeadm and also a video guide from Linux TV
Links: 
- https://www.youtube.com/watch?v=U1VzcjCB_sY&t
- https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/create-cluster-kubeadm/

I'm using this script in a virtual machine created in azure and my personal home lab.

When installing kubeadm make sure you create the dir "/etc/apt/keyrings". Only if the directory exists the command to install the gpg key will work.

When you initialize the cluster using the kubeadm init command , --control-plane-endpoint should be equal to either the DNS name or the Ip address of the loadbalancer if you have multiple control planes. Then for the --pod-network-cidr , its important that you have an overlay network , so for this flag we can assign the private ip address of any CNI plugin such as flannel or cillium ( which you should install after initializing the cluster). In this project the address is 10.244.0.0/16. This an important element for the cluster for pod networking in the cluster. 

(Run kubeadm init without the --control-plane-endpoint when you setup the cluster in azure or single home lab server with a single control plane)


Here is the documentation for the kubelet command: https://kubernetes.io/docs/reference/command-line-tools-reference/kubelet/ . 

Oncee you initialize the cluster using kubeadm you will find the coredns pods in the pending state and this is due to the pods waiting for something and that is the overlay network ( flannel in this case ) , so install it by applying the k8s manifests for flannel which can be found in the ReadMe file of their github: https://github.com/flannel-io/flannel


Issue regarding node not found in kubelet :https://github.com/kubernetes/kubeadm/issues/2370

