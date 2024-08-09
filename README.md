## K8s-at-home

This repository provides instructions for setting up rancher for Kubernetes cluster at home for learning and experimentation purposes. It supports both Windows and Linux systems.

**Prerequisites**

* **Machines:** At least one Linux (Ubuntu) machine and one Windows machine.
* **Docker:** Docker must be installed on both machines.
* **Remote Access:**
    * **Linux:** Ensure SSH access is configured.
    * **Windows:** Configure Windows Remote Management (WinRM).

**Installation**

# **Linux (Ubuntu)**

1. **Install Docker**

```
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
sudo apt-get install -y docker-ce
sudo usermod -aG docker $USER
```

2. **Install Rancher**

```
sudo docker run -d --restart=unless-stopped -p 80:80 -p 443:443 --name rancher rancher/rancher:latest
```

3. **Add Linux Node to Rancher**

```
sudo docker run -d --privileged --restart=unless-stopped --net=host -v /etc/kubernetes:/etc/kubernetes -v /var/run:/var/run -v /run:/run rancher/rancher-agent:v2.5.8 --server https://<RANCHER_SERVER> --token <TOKEN> --etcd --controlplane --worker
```

# **Windows**

1. **Install Docker Desktop**

Download and install Docker Desktop from the official Docker website.

2. **Configure Windows Node for Kubernetes**

   a. Download Kubernetes Node Components:

```
curl.exe -LO https://dl.k8s.io/v1.21.0/kubernetes-node-windows-amd64.tar.gz
tar -xvf kubernetes-node-windows-amd64.tar.gz
```

   b. Join the Rancher Cluster:

```
docker run -d --privileged --net=host --pid=host --volume=c:/run:/run rancher/rancher-agent:v2.5.8 --server https://<RANCHER_SERVER> --token <TOKEN> --worker
```

**Verify Cluster Nodes**

1. Access the Rancher UI and navigate to the "Clusters" section.
2. Check if both Linux and Windows nodes are listed as active.

**Deploy Workloads**

1. Use the Rancher UI to deploy applications (workloads).
2. Select appropriate container images based on the target operating system:
   * Use Ubuntu-based images for Linux workloads.
   * Use Windows-based images for Windows workloads.
3. Deploy your workload by navigating to "Cluster -> Workloads -> Deployments -> Deploy" in the Rancher UI.
4. Choose the appropriate namespaces and workload configurations for Windows and Linux environments.

**Access Workloads**

Access the deployed workloads using the endpoints and NodePort services provided by Rancher.

**Conclusion**

This guide demonstrates how to set up a Kubernetes cluster at home using Rancher to manage both Linux and Windows nodes. Rancher offers a user-friendly interface for managing multi-node and multi-OS clusters. Remember to adjust configurations as needed based on your specific network and system specifications.
