# ArgoCD ApplicationSet Demo

![Kubernets](https://img.shields.io/badge/-Kubernetes-326CE5?style=for-the-badge&logo=Kubernetes&logoColor=white)
![Helm](https://img.shields.io/badge/-Helm-0F1689?style=for-the-badge&logo=Helm&logoColor=white)
![Argo](https://img.shields.io/badge/-ArgoCD-fe733d?style=for-the-badge&logo=&logoColor=white)

This repository contains the examples and demos explained in the article
[How to create ArgoCD Applications Automatically using ApplicationSet? “Automation of the Gitops”](https://amralaayassen.medium.com/how-to-create-argocd-applications-automatically-using-applicationset-automation-of-the-gitops-59455eaf4f72)


---

<!-- END doctoc generated TOC please keep comment here to allow auto update -->
<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->


- [Prerequisites](#prerequisites)
    - [Kubernetes Cluster](#kubernetes-cluster)
    - [ArgoCD Installation](#argocd-installation)
    - [ArgoCD ApplicationSet Installation](#argocd-applicationset-installation)
    - [Add Repository to ArgoCD](#add-repository-to-argocd)
- [List Generator](#list-generator)
- [Git Directory](#git-directory)
- [Demo](#demo)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->
---

### Prerequisites

##### Kubernetes Cluster

To be able to run the examples and demos in this repository you need to have a running Kubernetes cluster with `argocd` namespace

you can install minikube locally if you don't have a cluster check [here](https://minikube.sigs.k8s.io/docs/start/)

you can also use a 2 nodes cluter using vagrant boxes check this [Github repo](https://github.com/theJaxon/Kontainerd)

##### ArgoCD Installation

you need to have argocd installed on your cluster in `argocd` namespace

```
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

or check the [documentation](https://argo-cd.readthedocs.io/en/stable/)

##### ArgoCD ApplicationSet Installation

you need to install ApplicationSet alongside with argocd, some examples in this repository need a version > 0.3 make sure to use the latest image in your ApplicationSet Deployment

```
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/applicationset/master/manifests/install.yaml
```

or check the [documentation](https://argocd-applicationset.readthedocs.io/en/stable/)

##### Add Repository to ArgoCD

ArgoCD needs access to this repository to be able to apply the demo examples, you can add this repository by applying the below manifest file on your `argocd` namepace

```yaml
apiVersion: v1
kind: Secret
metadata:
  namespace: argocd
  labels:
    argocd.argoproj.io/secret-type: repository
  annotations:
    managed-by: argocd.argoproj.io
data:
  project: ZGVmYXVsdA==
  type: Z2l0
  url: >-
    aHR0cHM6Ly9naXRodWIuY29tL0FtckFsYWFZYXNzZW4vQXJnb0NELUFwcGxpY2F0aW9uU2V0LURlbW8uZ2l0
type: Opaque
```

### List Generator

in this example we use ApplicationSet list generator to create same application to different destination (namespaces in our case) we have 3 different namespaces that need to be created as prerequisite to argo to be able to deploy those applications, namespaces are

- `uat`
- `dev`
- `test`

defined in the destination section in the application template section of the ApplicationSet manifest

```yaml
destination:
  # default cluster as destination, you can define multiple clusters in ArgoCD UI
  server: https://kubernetes.default.svc
  # will deploy to different namespaces as different destinations
  namespace: '{{namespace}}'
```

### Git Directory

in this example we install a list of argo projects each project will be deployed on a specific namespace named with the tool name

- argo-image-upater will be deployed on `argo-image-updater` namespace
- argo-rollouts will be deployed on `argo-rollouts` namespace
- argo-workflows will be deployed on `argo-workflows` namespace

### Demo

in this example we getting more advanced using Git Directory generator
we have a directory tree like the one below

```
           ├── app-hulk ────├── prod
           │                ├── qa
           │                ├── staging
           │
├── configs├── app-iron ────├── prod
│          │                ├── qa
│          │                ├── staging
│          │
│          ├── app-spider ──├── prod
│                           ├── qa
│                           ├── staging
│
├── templates # helm templates to be used
├── chart.yaml # our chart file
```
we have 3 different applications and we need to deploy them to 3 different environments `prod`, `staging`, and `qa` we have developed a common helm chart to be used for all of the 3 applications for each combination of application and environment we have different `values.yaml` files to be used alongside with our common helm chart, see diagram below
![architecture](https://github.com/AmrAlaaYassen/ArgoCD-ApplicationSet-Demo/blob/main/media/architecture.png)
