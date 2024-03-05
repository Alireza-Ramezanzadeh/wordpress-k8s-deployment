# WordPress Deployment on Kubernetes

This repository provides manifests and instructions for deploying WordPress on a Kubernetes cluster using persistent volumes. The deployment is based on the official Kubernetes tutorial for [MySQL and WordPress with Persistent Volumes](https://kubernetes.io/docs/tutorials/stateful-application/mysql-wordpress-persistent-volume/).

## Prerequisites

Before you begin, make sure you have the following:

- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) installed

## Installing Kind

1. **Download the Kind Binary:**
   Download the Kind binary from the [Kind releases page](https://github.com/kubernetes-sigs/kind/releases). You can use the following command for Linux:

    ```bash
    curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.12.0/kind-linux-amd64
    ```

   Make the binary executable:

    ```bash
    chmod +x ./kind
    ```

2. **Move the Binary to a Directory in Your PATH:**
   Move the `kind` binary to a directory in your PATH (e.g., `/usr/local/bin`):

    ```bash
    sudo mv ./kind /usr/local/bin/kind
    ```

## Creating a Kind Cluster

1. **Clone this repository:**
   ```bash
   git clone https://github.com/your-username/wordpress-k8s-deployment.git
   cd wordpress-k8s-deployment
   ```
2. **Create a Kind Cluster:**
   Create a local Kubernetes cluster using Kind. This will use Docker containers as nodes:

    ```bash
    kind  create cluster --name limoo-cluster --config=kind-cluster/mycluster.yml
    ```

4. **Set Kubeconfig Context:**
   Kind automatically updates your `kubectl` configuration. Verify the new context:

    ```bash
    kubectl config get-contexts
    ```

5. **Verify Cluster Status:**
   Check the status of your cluster:

    ```bash
    kubectl cluster-info
    ```

   It should show the Kubernetes master and core DNS services.

### Delete Kind Cluster

Use the following command to delete  Kind cluster named "limoo-cluster":

```bash
kind delete clusters limoo-cluster
```
## Getting Started with WordPress Deployment

1. Clone this repository:

    ```bash
    git clone https://github.com/your-username/wordpress-k8s-deployment.git
    cd wordpress-k8s-deployment
    ```

2. Deploy WordPress and MySQL:

    ```bash
    kubectl apply -f namespace.yaml
    kubectl apply -f manifests/
    ```

   This will deploy MySQL and WordPress applications along with persistent volumes.

3. Access WordPress in your web browser using the provided IP and port.

## Expose wordpress with public ip
Considering that our cluster is up with Kind and each of our nodes is a Docker container, we can expose WordPress with a public IP with the following commands:

1. find control plane node ip address container
```bash
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' limoo-cluster-control-plane
```
2. find NodePort:
```bash
kubectl get services wordpress -n wordpress
```
2. proxy with iptable:
``` bash
sysctl net.ipv4.ip_forward=1
iptables -A INPUT -i lo -j ACCEPT
iptables -A OUTPUT -o lo -j ACCEPT

iptables -t nat -A PREROUTING -p tcp --dport <web-port> -j DNAT --to-destination <control-plane-ip>:<NodePort>
iptables -t nat -A PREROUTING -p udp --dport <web-port> -j DNAT --to-destination <control-plane-ip>:<NodePort>

iptables -t nat -A POSTROUTING -j MASQUERADE
/sbin/iptables-save

```
## Cleaning Up

To delete the resources created by this deployment and remove the Kind cluster, run:

```bash
kubectl delete -f manifests/
kind delete clusters limoo-cluster
