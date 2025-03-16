# 🚀 Kubernetes Setup and Commands 🛠️

## 📥 Install Hyperkit and Minikube
```sh
brew update
brew install hyperkit
brew install minikube
kubectl
minikube
```

## 🔧 Create Minikube Cluster
```sh
minikube start --vm-driver=hyperkit
kubectl get nodes
minikube status
kubectl version
```

## 🗑️ Delete Cluster and Restart in Debug Mode
```sh
minikube delete
minikube start --vm-driver=hyperkit --v=7 --alsologtostderr
minikube status
```

## 📌 Basic kubectl Commands
```sh
kubectl get nodes
kubectl get pod
kubectl get services
kubectl create deployment nginx-depl --image=nginx
kubectl get deployment
kubectl get replicaset
kubectl edit deployment nginx-depl
```

## 🛠️ Debugging
```sh
kubectl logs {pod-name}
kubectl exec -it {pod-name} -- bin/bash
```

## 🗄️ Create Mongo Deployment
```sh
kubectl create deployment mongo-depl --image=mongo
kubectl logs mongo-depl-{pod-name}
kubectl describe pod mongo-depl-{pod-name}
```

## ❌ Delete Deployment
```sh
kubectl delete deployment mongo-depl
kubectl delete deployment nginx-depl
```

## 📝 Create or Edit Config File
```sh
vim nginx-deployment.yaml
kubectl apply -f nginx-deployment.yaml
kubectl get pod
kubectl get deployment
```

## 🗑️ Delete with Config
```sh
kubectl delete -f nginx-deployment.yaml
```

## 📊 Metrics Monitoring
```sh
kubectl top nodes
kubectl top pods
```

This file provides a structured reference for setting up and managing Minikube with Kubernetes. 🚀🎯
