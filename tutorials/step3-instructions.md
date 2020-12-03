Kubernetes Voting Example - Step #3
=========
In this step you need to create the Result microservice, which will present the results of the vote to end-users.

The application requires a recent version of the Result microservice, specifically dockersamples/examplevotingapp_result:before. Result should be accessible by other microservices behind your firewall. In addition, it should be accessible to end-users via port 31001.

Other microservices expect to reach the Result microservice with the name "result" via port 5001; however, the actual application listens on port 80.


Deployment
-----
Create a file (be aware of the identation):

```
result-deployment.yaml
```

Define metadata for deployment:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: result
  name: result
  namespace: vote
```
On this part you are defining deployment name, labels to identify it and the namespace where the deployment will be defined.

Then, start to define specification for your deployment:
```
spec:
  replicas: 1
  selector:
    matchLabels:
      app: result
```

On this you're stating that you want 1 replica of the pod and must have the label 'result' assinged to it.

After that, you need to specify your pod definition. This pod use the public available 'dockersamples/examplevotingapp_result:before' docker image and expose port 80:

```
template:
metadata:
    labels:
    app: result
spec:
    containers:
    - image: dockersamples/examplevotingapp_result:before
    name: result
    ports:
    - containerPort: 80
        name: result
```

Service
-----

Create a file (be aware of the identation):

```
result-service.yaml
```

Define metadata for service:
```
apiVersion: v1
kind: Service
metadata:
  labels:
    app: result
  name: result
  namespace: vote
```
On this part you are defining service name, labels to identify it and the namespace where the service will be defined.

Since it should be accessible to end-users via port 31001 and other microservices expect to reach the Result microservice with the name "result" via port 5001 now we need to create a service of type NodePort with all mapping:

```
spec:
  type: NodePort
  ports:
  - name: "result-service"
    port: 5001
    targetPort: 80
    nodePort: 31001
  selector:
    app: result
```

- targetPort, means exposed port by the pod/container
- port, means the port to be used internally by other resources inside the cluster
- nodePort, means the mapping with host port


Apply on Kubernetes cluster
-----

Execute the following commands and verify if everything is running properly:

```
kubectl apply -f result-deployment.yaml
kubectl apply -f result-service.yaml
```


Next step
-----

Let's proceed to the [Step #4: Vote microservice which will allow end-users to vote for their favorite pet](step4-instructions.md).
