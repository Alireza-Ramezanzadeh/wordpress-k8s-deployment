# WordPress Deployment on Kubernetes

This repository provides manifests and instructions for deploying WordPress on a Kubernetes cluster using persistent volumes. The deployment is based on the official Kubernetes tutorial for [MySQL and WordPress with Persistent Volumes](https://kubernetes.io/docs/tutorials/stateful-application/mysql-wordpress-persistent-volume/).

## Prerequisites

Before you begin, make sure you have the following:

- **kubectl:** Kubernetes command-line tool. Follow the steps in the README to install it.

## MySQL and WordPress Deployment

This tutorial demonstrates the deployment of WordPress and MySQL with persistent volumes on a Kubernetes cluster. Follow the steps below to deploy the required workloads:

1. **Clone this repository:**

    ```bash
    git clone https://github.com/your-username/wordpress-k8s-deployment.git
    cd wordpress-k8s-deployment
    ```

2. **Create a Kind Cluster:**
   If you haven't already, create a local Kubernetes cluster using Kind. This will use Docker containers as nodes:

    ```bash
    kind create cluster --name wordpress-cluster
    ```

3. **Set Kubeconfig Context:**
   Kind automatically updates your `kubectl` configuration. Verify the new context:

    ```bash
    kubectl config get-contexts
    ```

4. **Apply the MySQL and WordPress Manifests:**
   Deploy MySQL and WordPress applications along with persistent volumes using the manifests provided in the repository:

    ```bash
    kubectl apply -f manifests/
    ```

5. **Access WordPress:**
   Get the WordPress service IP:

    ```bash
    kubectl get svc wordpress
    ```

   Access WordPress in your web browser using the provided IP and port.

6. **Clean Up:**
   To delete the resources created by this deployment and remove the Kind cluster, run:

    ```bash
    kubectl delete -f manifests/
    kind delete cluster --name wordpress-cluster
    ```

## Contributing

Feel free to contribute to this project. Open issues or submit pull requests for any improvements or bug fixes.

## License

This project is licensed under the [MIT License](LICENSE).
