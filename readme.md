# ArgoCD ApplicationSet Demo

![Kubernets](https://img.shields.io/badge/-Kubernetes-326CE5?style=for-the-badge&logo=Kubernetes&logoColor=white)
![Helm](https://img.shields.io/badge/-Helm-0F1689?style=for-the-badge&logo=Helm&logoColor=white)
![Argo](https://img.shields.io/badge/-ArgoCD-fe733d?style=for-the-badge&logo=&logoColor=white)

This repository contains the examples and demos explaines in the article
[How to create ArgoCD Applications Automatically using ApplicationSet? “Automation of the Gitops”](https://amralaayassen.medium.com/how-to-create-argocd-applications-automatically-using-applicationset-automation-of-the-gitops-59455eaf4f72)

![architecture](https://github.com/AmrAlaaYassen/ArgoCD-ApplicationSet-Demo/blob/main/media/architecture.png)

---

<!-- END doctoc generated TOC please keep comment here to allow auto update -->
<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents** 

- [Prerequisites](#prerequisites)
        - [Kubernetes Cluster](#kubernetes-cluster)
        - [ArgoCD Installation](#argocd-installation)
        - [ArgoCD ApplicationSet Installation](#argocd-applicationset-installation)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

---
 
## Prerequisites
###### Kubernetes Cluster
To be able to run the examples and demos in this repository you need to have a running Kubernetes cluster with `argocd` namespace

you can install minikube locally if you don't have a cluster check [here](https://minikube.sigs.k8s.io/docs/start/)

you can also use a 2 nodes cluter using vagrant boxes check this [Github repo](https://github.com/theJaxon/Kontainerd)

###### ArgoCD Installation
you need to have argocd installed on your cluster in `argocd` namespace
```
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml 
``` 

or check the [documentation](https://argo-cd.readthedocs.io/en/stable/)

###### ArgoCD ApplicationSet Installation
you need to install ApplicationSet alongside with argocd, some examples in this repository need a version > 0.3 make sure to use the latest image in your ApplicationSet Deployment

```
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/applicationset/master/manifests/install.yaml
```

or check the [documentation](https://argocd-applicationset.readthedocs.io/en/stable/)
