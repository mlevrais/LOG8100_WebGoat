# WEBGOAT Application - Project

## Deployment

### Configure K8s cluster locally with Minikube

#### Starting Minikube

Start Docker Desktop, then start Minikube to create a VM and a Kubernetes cluster on that machine with the following command:

```
minikube start --driver=docker
```

If starting Minikube fails, try deleting with this command and retry again:

```
minikube delete
```

You can run this command to check the Minikube status

```
minikube status
```

References: [Starting minikube on Windows](https://stackoverflow.com/questions/71774813/minikube-fails-to-start-on-windows-11-home-and-docker-desktop), [Setup tutorial](https://medium.com/rahasak/replace-docker-desktop-with-minikube-and-hyperkit-on-macos-783ce4fb39e3)

#### Configure cluster

In your terminal, go to the `terraform` directory

```
cd ./terraform
```

In `variables.tfvars`, set the path to your `.kube/config` folder. It is where Kubernetes store its config files. On Windows, usually its `$env:userprofile/.kube/config`.
So in a powershell terminal, type `$env:userprofile` to get the first part of the path. If you are using `\` in the path, make sure to escape it with `\\`.
Then initialize Terraform

```
terraform init
```

Then apply the tf files to configure the cluster

```
terraform apply -var-file="variables.tfvars"
```

### Confirming that the cluster is configured

Use the following command to make sure the cluster is configured. You should see the namespace `webgoat`.

```
kubectl get namespaces
```

### Configure K8s cluster on Kubernetes

#### Requirements:

- Before configuring the cluster, you need to register an application on Google Console.
- Then you need to install Google Cloud SDK, and set up the project. The following command configures Google Cloud SDK to point to the project.

```
gcloud config set project ${{ secrets.PROJECT_ID }}
gcloud components install gke-gcloud-auth-plugin
gcloud container clusters get-credentials ${{ secrets.GKE_CLUSTER }} --region=${{ secrets.GKE_REGION }}
```

Then, in your terminal, go to the `terraform/gke` directory

```
cd ./terraform/gke
```

If you are running Terraform for the first time, intialize Terraform:

```
terraform init
```

If you updated the `variables.tfvars` file, apply the tf files to provision the cluster:

```
terraform apply
```

