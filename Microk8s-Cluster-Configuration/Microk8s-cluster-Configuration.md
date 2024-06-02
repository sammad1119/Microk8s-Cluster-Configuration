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