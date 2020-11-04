Kubernetes Voting Example - Step #5
=========
Finally, you need to create the Worker microservice, which will receive and then store votes in the database.

The application requires a recent version of the Worker microservice, specifically dockersamples/examplevotingapp_worker. The Worker microservice does not need to be accessible to end-users.

Deployment
-----
Create a file (be aware of the identation):

```
worker-deployment.yaml
```

Define metadata for deployment:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: worker
  name: worker
  namespace: vote
```
On this part you are defining deployment name, labels to identify it and the namespace where the deployment will be defined.

Then, start to define specification for your deployment:
```
spec:
  replicas: 1
  selector:
    matchLabels:
      app: worker
```

On this you're stating that you want 1 replica of the pod and must have the label 'worker' assinged to it.

After that, you need to specify your pod definition. This pod use the public available 'dockersamples/examplevotingapp_worker' docker image:

```
template:
metadata:
    labels:
    app: worker
spec:
    containers:
    - image: dockersamples/examplevotingapp_worker
    name: worker
```


Apply on Kubernetes cluster
-----

Execute the following commands and verify if everything is running properly:

```
kubectl apply -f worker-deployment.yaml
```


