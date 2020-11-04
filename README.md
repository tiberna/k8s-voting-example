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
* A .NET Core worker which consumes votes and stores them in database
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
- [Step #2](/tutorials/step2-instructions.md): Redis which will collect new votes for the application.
- [Step #3](/tutorials/step3-instructions.md): Result microservice which will present the results of the vote to end-users.
- [Step #4](/tutorials/step4-instructions.md): Vote microservice which will allow end-users to vote for their favorite pet.
- [Step #5](/tutorials/step5-instructions.md): Worker microservice which will receive and then store votes in the database.

Now, you should have a total of 5 Deployments and 4 Services at minimum. Create these objects using the Kubernetes CLI.

At this point, you should be able to access the Voting App via port 31000 and 31001. However, if it isn't working properly, you need to debug.

Check the status of all Services and Deployments. Read the logs from all of your Pods. Double-check configuration specified in all steps of the lab.

- [Step #6](/tutorials/step6-instructions.md): Interacting with the Voting App


Resources
-----
Original repo for Voting App: https://github.com/dockersamples/example-voting-app

Kubernetes basics: 