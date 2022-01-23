# Install Kubernetes

## Prerequisites

- 2 GB or more of RAM per machine (any less will leave little room for your apps).
- 2 CPUs or more.
- Full network connectivity between all machines in the cluster (public or private network is fine).
- Unique hostname, MAC address, and product_uuid for every node.
- Certain ports are open on your machines.

## **Do this on both kMaster and kWorker:**

### 1. Diable Swap:

```bash
sudo -i
swapoff -a; sed -i '/swap/d' /etc/fstab
exit
```

### 2. Install Containerd Engine

- Install pre-requisites for containerd
    
    ```bash
    sudo apt-get update \
        && sudo apt-get install -y apt-transport-https \
            ca-certificates curl software-properties-common
    ```
    
- Load modules overlay and br_netfilter
    
    ```bash
    cat <<EOF | sudo tee /etc/modules-load.d/containerd.conf
    overlay
    br_netfilter
    EOF
    
    sudo modprobe overlay \
        && sudo modprobe br_netfilter
    ```
    
- Set sysctl parameters and reload conf files
    
    ```bash
    cat <<EOF | sudo tee /etc/sysctl.d/99-kubernetes-cri.conf
    net.bridge.bridge-nf-call-iptables  = 1
    net.ipv4.ip_forward                 = 1
    net.bridge.bridge-nf-call-ip6tables = 1
    EOF
    
    sudo sysctl --system
    ```
    
- Setup the Docker repo
    
    ```bash
    curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key --keyring /etc/apt/trusted.gpg.d/docker.gpg add -
    sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu \$(lsb_release -cs) \stable"
    ```
    
- Install containerd
    
    ```bash
    sudo apt-get update \
    && sudo apt-get install -y containerd.io
    ```
    
- Create default config.toml
    
    ```bash
    sudo mkdir -p /etc/containerd \
    && sudo containerd config default | sudo tee /etc/containerd/config.toml
    ```
    
- Configure containerd to use systemd cgroup drive with runc by editing conf file:
    
    ```bash
    sudo nano /etc/containerd/config.toml
    ```
    
    adding this line:
    
    ```bash
    [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
    SystemdCgroup = true
    ```
    
- Restart daemon
    
    ```bash
    sudo systemctl restart containerd
    ```
    

## **Do this on kMaster:**

- Initialisation with kubeadm for cluster network in intern network, in the further steps a Calico network is used
    
    ```bash
    kubeadm init --pod-network-cidr=192.168.0.0/16
    ```
    
    <aside>
    ℹ️ If 192.168.0.0/16 is already in use within your network you must select a different pod network CIDR, replacing 192.168.0.0/16 in the above command.
    
    </aside>
    
- Finish conf setup
    
    ```bash
    mkdir -p $HOME/.kube \
    && sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config \
    && sudo chown $(id -u):$(id -g) $HOME/.kube/config
    ```
    
- Deploy Calico Network
    
    ```bash
    sudo kubectl --kubeconfig=/etc/kubernetes/admin.conf create -f https://docs.projectcalico.org/v3.14/manifests/calico.yaml
    sudo kubectl create -f [https://docs.projectcalico.org/manifests/tigera-operator.yaml](https://docs.projectcalico.org/manifests/tigera-operator.yaml)
    ```
    
- Get cluster join command
    
    ```bash
    kubeadm token create --print-join-command
    ```
    
- Get Cluster Nodes
    
    ```bash
    kubectl get nodes
    ```
    

## **Do this on kWorker:**

- Copy and Pate the "Get cluster join command", simliar to:
    
    ```bash
    kubeadm join 10.0.2.7:6443 --token e2ctwm.barg1e4w35lg4kej --discovery-token-ca-cert-hash sha256:ea81b70854c915a8d965b038f897261affc4889398b46e1cea3b3f1fddd95036
    ```
    
    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/f97ae000-19b9-4e5d-ac2d-0a5ee6ef997c/Untitled.png)
