https://github.com/kubernetes/minikube
> Minikube is a tool that makes it easy to run Kubernetes locally. Minikube runs a single-node Kubernetes cluster inside a VM on your laptop for users looking to try out Kubernetes or develop with it day-to-day.


## Installation
### Mac OS
```
curl -Lo minikube https://storage.googleapis.com/minikube/releases/v0.14.0/minikube-darwin-amd64 && chmod +x minikube && sudo mv minikube /usr/local/bin/  
curl -Lo kubectl https://storage.googleapis.com/kubernetes-release/release/v1.5.1/bin/darwin/amd64/kubectl && chmod +x kubectl && sudo mv kubectl /usr/local/bin/
```


## Examples
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


## Commands
```
Usage:
  minikube [command]

Available Commands:
  addons           Modify minikube's kubernetes addons
  completion       Outputs minikube shell completion for the given shell (bash)
  config           Modify minikube config
  dashboard        Opens/displays the kubernetes dashboard URL for your local cluster
  delete           Deletes a local kubernetes cluster.
  docker-env       sets up docker env variables; similar to '$(docker-machine env)'
  get-k8s-versions Gets the list of available kubernetes versions available for minikube.
  ip               Retrieve the IP address of the running cluster.
  logs             Gets the logs of the running localkube instance, used for debugging minikube, not user code.
  service          Gets the kubernetes URL(s) for the specified service in your local cluster
  ssh              Log into or run a command on a machine with SSH; similar to 'docker-machine ssh'
  start            Starts a local kubernetes cluster.
  status           Gets the status of a local kubernetes cluster.
  stop             Stops a running local kubernetes cluster.
  version          Print the version of minikube.

Flags:
      --alsologtostderr                  log to standard error as well as files
      --log_backtrace_at traceLocation   when logging hits line file:N, emit a stack trace (default :0)
      --log_dir string                   If non-empty, write log files in this directory (default "")
      --logtostderr                      log to standard error instead of files
      --show-libmachine-logs             Whether or not to show logs from libmachine.
      --stderrthreshold severity         logs at or above this threshold go to stderr (default 2)
  -v, --v Level                          log level for V logs
      --vmodule moduleSpec               comma-separated list of pattern=N settings for file-filtered logging

Use "minikube [command] --help" for more information about a command.
```
