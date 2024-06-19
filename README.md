# K8-at-home
This repo helps you spin up kubernetes at home for practice i given instructions for both windows and linux

To set up a home Kubernetes cluster using Rancher for managing Docker containers on both Windows and Linux (Ubuntu) systems, follow these steps. Rancher is a powerful Kubernetes management platform that simplifies cluster deployment and management. Below is a guide that includes the installation of Rancher and setting up a Kubernetes cluster with both Linux and Windows nodes.

# Prerequisites
Linux and Windows machines: Ensure you have at least one Linux (Ubuntu) and one Windows machine.
Docker installed: Docker should be installed on both Linux and Windows machines.
SSH access: For the Linux machine, ensure SSH access is set up. For the Windows machine, ensure WinRM (Windows Remote Management) is configured.
Step-by-Step Guide
1. Install Docker on Ubuntu
On your Ubuntu machine, install Docker:
bash
```
sudo apt-get update
sudo apt-get install -y apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
sudo apt-get install -y docker-ce
sudo usermod -aG docker $USER
```
2. Install Docker on Windows
Download and install Docker Desktop from the Docker website.

3. Install Rancher on Ubuntu
On your Ubuntu machine, run the following commands to install Rancher:
```
Copy code
sudo docker run -d --restart=unless-stopped -p 80:80 -p 443:443 --name rancher rancher/rancher:latest
```
After Rancher is up and running, access it via https://<Ubuntu_IP>. Follow the setup instructions to create an admin user and set up the Rancher server.
4. Add Linux Node to Rancher
In Rancher, add a new cluster:

Navigate to `Global -> Clusters -> Add Cluster.`
Select From existing nodes (Custom).
Choose Kubernetes version and click Next.
For the Linux node, select etcd, Control Plane, and Worker roles.
Copy the generated Docker command and run it on your Ubuntu machine.
Example command:

bash
```
sudo docker run -d --privileged --restart=unless-stopped --net=host -v /etc/kubernetes:/etc/kubernetes -v /var/run:/var/run -v /run:/run rancher/rancher-agent:v2.5.8 --server https://<RANCHER_SERVER> --token <TOKEN> --etcd --controlplane --worker
```
5. Configure Windows Node for Kubernetes
Install Docker on Windows
Ensure Docker Desktop is installed and Kubernetes is enabled.

Install Kubernetes Node Components
Download and install Kubernetes node components:

powershell
```
curl.exe -LO https://dl.k8s.io/v1.21.0/kubernetes-node-windows-amd64.tar.gz
tar -xvf kubernetes-node-windows-amd64.tar.gz
```
Set up the node to join the Rancher-managed cluster:

Open a PowerShell prompt as Administrator and run the Rancher agent command provided by the Rancher UI for the Windows node.
Example command:

powershell
```
docker run -d --privileged --net=host --pid=host --volume=c:/run:/run rancher/rancher-agent:v2.5.8 --server https://<RANCHER_SERVER> --token <TOKEN> --worker
```
6. Verify Cluster Nodes
Go to Clusters in the Rancher UI.
Check if both Linux and Windows nodes are listed and active.
7. Deploy Workloads
Use the Rancher UI to deploy workloads.
For Linux workloads, use Ubuntu-based images.
For Windows workloads, use Windows-based images.
Example workload deployment:

Navigate to` Cluster -> Workloads -> Deployments -> Deploy.`
Select appropriate namespaces and workloads for Windows and Linux.
8. Access Workloads
Access the workloads using the provided endpoints and NodePort services configured in Rancher.
Conclusion
This guide sets up a home Kubernetes cluster using Rancher to manage both Linux (Ubuntu) and Windows nodes. Rancher provides an easy-to-use interface for managing multi-node and multi-OS clusters. Adjust configurations as needed based on your home network and system specifications.
