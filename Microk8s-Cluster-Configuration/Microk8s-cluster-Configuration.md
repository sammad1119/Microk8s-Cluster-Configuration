## Microk8s Cluster configuration and Setup


## prerequisites

- Hardware Requirements

1. CPU: At least 2 cores.
2. RAM: Minimum 4GB of RAM.
3. Storage: At least 20GB of disk space.
4. Software Requirements

- Operating System:

Ubuntu 18.04 LTS or later is recommended.
Other Linux distributions can be used, but Ubuntu offers the best compatibility.

- Supported Architectures:

x86_64 (Intel/AMD)
ARM (Raspberry Pi, etc.)

- Container Runtime:

MicroK8s includes its own container runtime, so you don't need Docker or any other runtime pre-installed.

- Network:

1. Ensure you have a reliable network connection.
2. Firewall settings should allow traffic on the following ports:
3. 16443/tcp for Kubernetes API server.
4. 10250/tcp for Kubelet API.
5. 8472/udp for VXLAN (used for inter-node communication).
6. 30000-32767/tcp for NodePort Services.

- System Configuration

1. Snapd:

MicroK8s is distributed as a snap package. Ensure snapd is installed and running on your system.

```
sudo apt update
sudo apt install snapd
sudo systemctl enable --now snapd

```
- Swap Space:

It is recommended to disable swap to ensure Kubernetes functions properly.

```
sudo swapoff -a
sudo sed -i '/ swap / s/^/#/' /etc/fstab

```
- Optional Dependencies

1. Helm:
Helm is a package manager for Kubernetes and can be useful for managing applications on your cluster.
2. kubectl:
Although MicroK8s includes its own kubectl, you might want to install the standalone version for convenience.
```
sudo snap install kubectl --classic

```
## Install MicroK8s


```
sudo snap install microk8s --classic 

```

## Join the Group 

- MicroK8s creates a group to enable seamless usage of commands which require admin privilege. To add your current user to the group and gain access to the .kube caching directory, run the following three commands:

```
sudo usermod -a -G microk8s $USER
mkdir -p ~/.kube
chmod 0700 ~/.kube
```
- You will also need to re-enter the session for the group update to take place:

```
su - $USER

```
## Check the status

- MicroK8s has a built-in command to display its status. During installation you can use the --wait-ready flag to wait for the 

Kubernetes services to initialise:

```
microk8s status --wait-ready
```
## Access Kubernetes

- MicroK8s bundles its own version of kubectl for accessing Kubernetes. Use it to run commands to monitor and control your Kubernetes. 

For example, to view your node:

```
microk8s kubectl get nodes
```
- To see the running services:
```
microk8s kubectl get services
```
MicroK8s uses a namespaced kubectl command to prevent conflicts with any existing installs of kubectl. If you don’t have an existing install, it is easier to add an alias (append to ~/.bash_aliases) like this:

alias kubectl='microk8s kubectl'

## Deploy an app

- Of course, Kubernetes is meant for deploying apps and services. You can use the kubectl command to do that as with any Kubernetes. Try installing a demo app:
```
microk8s kubectl create deployment nginx --image=nginx
```
- It may take a minute or two to install, but you can check the status:
```
microk8s kubectl get pods
```
## Use add-ons

1. MicroK8s uses the minimum of components for a pure, lightweight Kubernetes. However, plenty of extra features are available with a few keystrokes using “add-ons” - pre-packaged components that will provide extra capabilities for your Kubernetes, from simple DNS management to machine learning with Kubeflow!

2. To start it is recommended to add DNS management to facilitate communication between services. For applications which need storage, the ‘hostpath-storage’ add-on provides directory space on the host. 

3. These are easy to set up:
```
microk8s enable dns
microk8s enable hostpath-storage
```
4. See the full list of addons 

```
1. https://microk8s.io/docs/addons

```
## Starting and Stopping MicroK8s

- MicroK8s will continue running until you decide to stop it. You can stop and start MicroK8s with these simple commands:
```
microk8s stop
```
 - will stop MicroK8s and its services. You can start again any time by running:
```
microk8s start
```
Note that if you leave MicroK8s running, it will automatically restart after a reboot. If you don’t want this to happen, simply remember to run microk8s stop before you power down.

## Adding a node

- To create a cluster out of two or more already-running MicroK8s instances, use the microk8s add-node command. The MicroK8s instance on which this command is run will be the master of the cluster and will host the Kubernetes control plane:
```
microk8s add-node
```
- This will return some joining instructions which should be executed on the MicroK8s instance that you wish to join to the cluster (NOT THE NODE YOU RAN add-node FROM)

- From the node you wish to join to this cluster, run the following:

```
Depend on your outafter runing the above add-node command

microk8s join 192.168.1.230:25000/92b2db237428470dc4fcfc4ebbd9dc81/2c0cb3284b05

```
- For joining a node as a Worker.

```
Use the '--worker' flag to join a node as a worker not running the control plane, eg:

microk8s join 192.168.1.230:25000/92b2db237428470dc4fcfc4ebbd9dc81/2c0cb3284b05 --worker
```

- If the node you are adding is not reachable through the default interface you can use one of the following:
```
microk8s join 192.168.1.230:25000/92b2db237428470dc4fcfc4ebbd9dc81/2c0cb3284b05
microk8s join 10.23.209.1:25000/92b2db237428470dc4fcfc4ebbd9dc81/2c0cb3284b05
microk8s join 172.17.0.1:25000/92b2db237428470dc4fcfc4ebbd9dc81/2c0cb3284b05
```
- Joining a node to the cluster should only take a few seconds. Afterwards you should be able to see the node has joined:

```
microk8s kubectl get nodes

NAME               STATUS   ROLES    AGE   VERSION
10.22.254.79       Ready    <none>   27s   v1.15.3
ip-172-31-20-243   Ready    <none>   53s   v1.15.3

```
## Removing a node

- First, on the node you want to remove, run microk8s leave. MicroK8s on the departing node will restart its own control plane and resume operations as a full single node cluster:
```
microk8s leave

```
- To complete the node removal, call microk8s remove-node from the remaining nodes to indicate that the departing (unreachable now) node should be removed permanently:

```
Ip of your node below is the example Ip given 
microk8s remove-node 10.22.254.79
```


## For more detail 

1. https://microk8s.io/docs
2. https://microk8s.io/docs/getting-started
