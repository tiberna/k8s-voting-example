Kubernetes Voting Example - Step #4
=========
In this step you need to create the Vote microservice, which will allow end-users to vote for their favorite pet.

The application requires a recent version of the Vote microservice, specifically dockersamples/examplevotingapp_vote:before. Vote should be accessible by other microservices behind your firewall, specifically those that are part of the Voting App. In addition, it should be accessible to end-users via port 31000.

Other microservices expect to reach the Vote microservice with the name "vote" via port 5000; however, the actual listens on port 80.

Deployment
-----
Create a file (be aware of the identation):

```
vote-deployment.yaml
```

Define metadata for deployment:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: vote
  name: vote
  namespace: vote
```
On this part you are defining deployment name, labels to identify it and the namespace where the deployment will be defined.

Then, start to define specification for your deployment:
```
spec:
  replicas: 1
  selector:
    matchLabels:
      app: vote
```

On this you're stating that you want 1 replica of the pod and must have the label 'result' assinged to it.

After that, you need to specify your pod definition. This pod use the public available 'dockersamples/examplevotingapp_vote:before' docker image and expose port 80:

```
template:
metadata:
    labels:
    app: vote
spec:
    containers:
    - image: dockersamples/examplevotingapp_vote:before
    name: vote
    ports:
    - containerPort: 80
        name: vote
```

Service
-----

Create a file (be aware of the identation):

```
vote-service.yaml
```

Define metadata for service:
```
apiVersion: v1
kind: Service
metadata:
  labels:
    app: vote
  name: vote
  namespace: vote
```
On this part you are defining service name, labels to identify it and the namespace where the service will be defined.

Since it should be accessible to end-users via port 31000 and other microservices expect to reach the Result microservice with the name "result" via port 5000 now we need to create a service of type NodePort with all mapping:

```
spec:
  type: NodePort
  ports:
  - name: "vote-service"
    port: 5000
    targetPort: 80
    nodePort: 31000
  selector:
    app: vote
```

- targetPort, means exposed port by the pod/container
- port, means the port to be used internally by other resources inside the cluster
- nodePort, means the mapping with host port


Apply on Kubernetes cluster
-----

Execute the following commands and verify if everything is running properly:

```
kubectl apply -f vote-deployment.yaml
kubectl apply -f vote-service.yaml
```


