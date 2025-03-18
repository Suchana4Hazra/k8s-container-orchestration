# Kubernetes Namespaces: A Hands-on Guide

Namespaces in Kubernetes provide a mechanism for isolating groups of resources within a single cluster. This guide will walk you through everything you need to know about creating, managing, and working with namespaces using kubectl.

## Prerequisites

- Kubernetes cluster (local or remote)
- kubectl installed and configured to access your cluster
- Basic understanding of Kubernetes concepts

## What are Namespaces?

Namespaces are virtual clusters within a physical Kubernetes cluster. They help you:

- Organize resources in a multi-tenant environment
- Separate concerns between teams, projects, or environments
- Apply resource quotas and access controls at a namespace level

## Getting Started with Namespaces

### View Existing Namespaces

Let's start by listing all existing namespaces in your cluster:

```bash
kubectl get namespaces
```

This will display output similar to:

```
NAME              STATUS   AGE
default           Active   45d
kube-node-lease   Active   45d
kube-public       Active   45d
kube-system       Active   45d
```

### Namespace Details

To get more detailed information about a specific namespace:

```bash
kubectl describe namespace default
```

### Create a New Namespace

You can create namespaces in two ways:

#### Method 1: Using kubectl create command

```bash
kubectl create namespace my-namespace
```

#### Method 2: Using a YAML manifest

Create a file named `namespace.yaml`:

```yaml
apiVersion: v1
kind: Namespace
metadata:
  name: my-namespace
```

Then apply it:

```bash
kubectl apply -f namespace.yaml
```

### Set a Namespace for kubectl Context

To avoid specifying the namespace in every command, you can set a default namespace for your current context:

```bash
kubectl config set-context --current --namespace=my-namespace
```

To verify the change:

```bash
kubectl config view --minify | grep namespace
```

## Working with Resources in Namespaces

### Create Resources in a Namespace

When creating resources, you can specify the namespace:

```bash
kubectl run nginx --image=nginx --namespace=my-namespace
```

Or for resources defined in YAML files:

```bash
kubectl apply -f deployment.yaml --namespace=my-namespace
```

### List Resources in a Specific Namespace

```bash
kubectl get pods --namespace=my-namespace
```

### Working Across All Namespaces

To list resources across all namespaces:

```bash
kubectl get pods --all-namespaces
# or the shorter form
kubectl get pods -A
```

## Hands-on Exercise: Managing Development and Production Environments

Let's create and manage resources in separate namespaces for development and production environments.

### Step 1: Create the Namespaces

```bash
kubectl create namespace dev
kubectl create namespace prod
```

### Step 2: Deploy an Application to Both Environments

Create a deployment.yaml file:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 2
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.19
        ports:
        - containerPort: 80
```

Deploy to both namespaces:

```bash
kubectl apply -f deployment.yaml --namespace=dev
kubectl apply -f deployment.yaml --namespace=prod
```

### Step 3: Scale the Deployments Differently

```bash
# Scale dev to 1 replica
kubectl scale deployment nginx-deployment --replicas=1 --namespace=dev

# Scale prod to 3 replicas
kubectl scale deployment nginx-deployment --replicas=3 --namespace=prod
```

### Step 4: Verify the Deployments

```bash
kubectl get deployments --namespace=dev
kubectl get deployments --namespace=prod
```

### Step 5: Create a Service in Each Namespace

Create a service.yaml file:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
  - port: 80
    targetPort: 80
  type: ClusterIP
```

Apply to both namespaces:

```bash
kubectl apply -f service.yaml --namespace=dev
kubectl apply -f service.yaml --namespace=prod
```

## Advanced Namespace Features

### Resource Quotas

You can limit resources in a namespace using ResourceQuota objects:

Create quota.yaml:

```yaml
apiVersion: v1
kind: ResourceQuota
metadata:
  name: compute-quota
spec:
  hard:
    pods: "10"
    requests.cpu: "4"
    requests.memory: 5Gi
    limits.cpu: "10"
    limits.memory: 10Gi
```

Apply it:

```bash
kubectl apply -f quota.yaml --namespace=dev
```

### Namespace Deletion

To delete a namespace and all resources within it:

```bash
kubectl delete namespace my-namespace
```

⚠️ **Warning**: This will delete ALL resources in the namespace!

## Namespace Best Practices

1. **Use namespaces for isolation**: Separate different environments, teams, or applications.
2. **Apply resource quotas**: Prevent one namespace from consuming all cluster resources.
3. **Implement RBAC**: Use Role-Based Access Control to restrict access to namespaces.
4. **Set default namespaces per context**: Configure kubectl contexts for different namespaces.
5. **Label resources consistently**: Apply consistent labels across namespaces for better organization.

## Troubleshooting Namespace Issues

### Cannot Delete Namespace (Stuck in "Terminating" State)

If a namespace gets stuck in the "Terminating" state:

```bash
kubectl get namespace my-namespace -o json > tmp.json
```

Edit tmp.json and remove the "finalizers" section, then:

```bash
kubectl replace --raw "/api/v1/namespaces/my-namespace/finalize" -f tmp.json
```

## Conclusion

You've now learned how to effectively work with Kubernetes namespaces using kubectl. Namespaces are powerful tools for organizing and isolating resources within your Kubernetes cluster. By applying these concepts in real-world scenarios, you can better manage multi-tenant environments and implement proper resource isolation between different teams or applications.

Happy Kuberneting!
