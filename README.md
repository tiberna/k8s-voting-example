Kubernetes Voting Example
=========

Using [Voting App](https://github.com/dockersamples/example-voting-app) to create a step-by-step tutorial to use Kubernetes for a full solution deploy.

Getting started
---------------

Have access to a Kubernetes cluster (AKS, EKS, GKE, Docker Desktop with Kubernetes enabled, minikube) and [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) configured

Architecture
-----

![Architecture diagram](architecture.png)

* A front-end web app in [Python](/vote) or [ASP.NET Core](/vote/dotnet) which lets you vote between two options
* A [Redis](https://hub.docker.com/_/redis/) or [NATS](https://hub.docker.com/_/nats/) queue which collects new votes
* A [.NET Core](/worker/src/Worker), [Java](/worker/src/main) or [.NET Core 2.1](/worker/dotnet) worker which consumes votes and stores them in…
* A [Postgres](https://hub.docker.com/_/postgres/) or [TiDB](https://hub.docker.com/r/dockersamples/tidb/tags/) database backed by a Docker volume
* A [Node.js](/result) or [ASP.NET Core SignalR](/result/dotnet) webapp which shows the results of the voting in real time

Steps
-----

Each step is configured on a separate branch on this repo. 



