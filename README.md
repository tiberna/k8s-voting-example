Kubernetes Voting Example
=========

Using [Voting App](https://github.com/dockersamples/example-voting-app) to create a step-by-step tutorial to use Kubernetes for a full solution deploy.

Getting started
---------------

Have access to a Kubernetes cluster (AKS, EKS, GKE, Docker Desktop with Kubernetes enabled, minikube) and [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) configured

Architecture
-----

![Architecture diagram](architecture.png)

* A front-end web app in [Python](/vote) which lets you vote between two options
* A [Redis](https://hub.docker.com/_/redis/) queue which collects new votes
* A [.NET Core](/worker/src/Worker) worker which consumes votes and stores them in…
* A [Postgres](https://hub.docker.com/_/postgres/) database backed by a Docker volume
* A [Node.js](/result) webapp which shows the results of the voting in real time

Instructions
-----
First, you must create a new namespace to handle all the resources that will be created during this lab.

```
kubectl create namespace vote
```
Then you may follow two tutorials:
## Hard way :)
Follow this [overview instructions] () for each step and you need to create the files by your own

## Step-by-step instructions
- [Step #1](https://github.com/tiberna/k8s-voting-example/blob/step1/step1-instructions.md): [Postgres](https://hub.docker.com/_/postgres/) database backed by a Docker volume
