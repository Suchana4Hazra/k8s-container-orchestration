# Kubernetes Commands Reference

## kubectl apply commands (in order)
Use these commands to create and update resources in your Kubernetes cluster:

```
kubectl apply -f mongo-secret.yaml
kubectl apply -f mongo.yaml
kubectl apply -f mongo-configmap.yaml 
kubectl apply -f mongo-express.yaml
```

## kubectl get commands
These commands help you view resources in your cluster:

```
kubectl get pod
kubectl get pod --watch
kubectl get pod -o wide
kubectl get service
kubectl get secret
kubectl get all | grep mongodb
```

## kubectl debugging commands
Use these commands to troubleshoot issues in your Kubernetes cluster:

```
kubectl describe pod mongodb-deployment-xxxxxx
kubectl describe service mongodb-service
kubectl logs mongo-express-xxxxxx
```

## Accessing External Services in Minikube
To access an external service in Minikube, use this command:

```
minikube service mongo-express-service
```

> **Note:** Replace `xxxxxx` with the actual pod name suffix from your deployment. You can find the full pod name using `kubectl get pods`.

---

*This reference guide covers basic commands for managing a MongoDB and Mongo Express deployment in Kubernetes.*
