# kind-nginx-ingress
KIND Nginx Ingress

# Start KIND

Refer : https://kind.sigs.k8s.io/docs/user/quick-start/

## Prereq

Make sure you create KIND cluster with `extraPortMappings` and `node-labels` 

Add the `extraPortMappings` to allow the requests on port `80` and `443`

```
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  kubeadmConfigPatches:
  - |
    kind: InitConfiguration
    nodeRegistration:
      kubeletExtraArgs:
        node-labels: "ingress-ready=true"
  extraPortMappings:
  - containerPort: 80
    hostPort: 80
    protocol: TCP
  - containerPort: 443
    hostPort: 443
    protocol: TCP
```

Refer : https://kind.sigs.k8s.io/docs/user/ingress/#create-cluster

# Nginx Ingress

Install Kubernetes Community Nginx Ingress Controller.

Refer : https://kind.sigs.k8s.io/docs/user/ingress/#ingress-nginx

## Steps:

Install Nginx Ingress Controller

```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
```

# Environment

Create a sample micro-services environment.


```
kubectl apply -f main-app.yaml
kubectl apply -f foo-app.yaml
kubectl apply -f bar-app.yaml
kubectl apply -f ext-app.yaml
kubectl apply -f rxz-app.yaml
kubectl apply -f example-ingress.yaml
kubectl apply -f new-ingress.yaml
```

# Test

Read : https://stackoverflow.com/questions/63232298/how-to-redirect-url-based-on-http-header-in-nginx-ingress

```
curl http://localhost/

curl http://localhost/foo

curl http://localhost/bar

curl http://localhost/ext

curl http://localhost/rxz

curl -H 'LocationHeader:EXT' http://localhost/

curl -H 'LocationHeader:RXZ' http://localhost/
