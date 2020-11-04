Kubernetes Voting Example
=========

Using [Voting App](https://github.com/dockersamples/example-voting-app) to create a step-by-step tutorial to use Kubernetes for a full solution deploy.

Getting started
---------------

Have access to a Kubernetes cluster (AKS, EKS, GKE, Docker Desktop with Kubernetes enabled, minikube) and [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) configured

Architecture
-----

![Architecture diagram](/files/architecture.png)

* A front-end web app in Python which lets you vote between two options
* A [Redis](https://hub.docker.com/_/redis/) queue which collects new votes
* A .NET Core worker which consumes votes and stores them in…
* A [Postgres](https://hub.docker.com/_/postgres/) database backed by a Docker volume
* A Node.js webapp which shows the results of the voting in real time

Instructions
-----
First, you must create a new namespace to handle all the resources that will be created during this lab.

```
kubectl create namespace vote
```
Then you may follow two tutorials:
### Hard way :)
Follow this [overview instructions](/tutorials/hard-way-tutorial.md) for each step and you need to create the files by your own

### Step-by-step instructions
- [Step #1](/tutorials/step1-instructions.md): Postgres database backed by a Docker volume
