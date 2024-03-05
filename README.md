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

1. **Create a Kind Cluster:**
   Create a local Kubernetes cluster using Kind. This will use Docker containers as nodes:

    ```bash
    kind create cluster --name wordpress-cluster
    ```

2. **Set Kubeconfig Context:**
   Kind automatically updates your `kubectl` configuration. Verify the new context:

    ```bash
    kubectl config get-contexts
    ```

3. **Verify Cluster Status:**
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
    kubectl apply -f manifests/
    ```

   This will deploy MySQL and WordPress applications along with persistent volumes.

3. Access WordPress in your web browser using the provided IP and port.

## Cleaning Up

To delete the resources created by this deployment and remove the Kind cluster, run:

```bash
kubectl delete -f manifests/
kind delete clusters limoo-cluster
