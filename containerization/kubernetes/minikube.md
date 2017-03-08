https://github.com/kubernetes/minikube
> Minikube is a tool that makes it easy to run Kubernetes locally. Minikube runs a single-node Kubernetes cluster inside a VM on your laptop for users looking to try out Kubernetes or develop with it day-to-day.


## Installation
### Mac OS
```
curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.14.0/minikube-darwin-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/  
curl -Lo kubectl https://storage.googleapis.com/kubernetes-release/release/v1.5.1/bin/darwin/amd64/kubectl && chmod +x kubectl && sudo mv kubectl /usr/local/bin/
```


## Example
```
$ minikube start / stop
$ minikube status
$ kubectl get cs
$ minikube docker-env

$ kubectl run hello-minikube --image=gcr.io/google_containers/echoserver:1.4 --port=8080  
$ kubectl expose deployment hello-minikube --type=NodePort

# check whether the pod is up and running
$ kubectl get pod

$ minikube service hello-minikube --url
```
