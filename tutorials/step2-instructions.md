Kubernetes Voting Example - Step #2
=========
Now you need to create Redis, which will collect new votes for the application.

The application requires the most recent minimal version of Redis. Redis should only be accessible by other microservices behind your firewall. It listens for connections on port 6379. Redis stores its data in /data. It doesn't require network-based persistent storage, a local directory mount will do.

Finally, other microservices expect to reach this microservice with the name "redis" on port 6379.


Deployment
-----
Create a file (be aware of the identation):

```
redis-deployment.yaml
```

Define metadata for deployment:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: redis
  name: redis
  namespace: vote
```
On this part you are defining deployment name, labels to identify it and the namespace where the deployment will be defined.

Then, start to define specification for your deployment:
```
spec:
  replicas: 1
  selector:
    matchLabels:
      app: redis
```

On this you're stating that you want 1 replica of the pod and must have the label 'redis' assinged to it.

After that, you need to specify your pod definition. This pod use the public available [Redis](https://hub.docker.com/_/redis) docker images with tag alpine and expose port 6379:

```
template:
  metadata:
    labels:
    app: redis
  spec:
    containers:
    - image: redis:alpine
    name: redis
    ports:
    - containerPort: 6379
        name: redis
```

The last step is to configure the volume inside 'spec' block:
```
volumes:
- name: redis-data
  emptyDir: {} 
```

And define the volume mount on the container:
```
volumeMounts:
- mountPath: /data
  name: redis-data
```

Service
-----

Create a file (be aware of the identation):

```
redis-service.yaml
```

Define metadata for service:
```
apiVersion: v1
kind: Service
metadata:
  labels:
    app: redis
  name: redis
  namespace: vote
```
On this part you are defining service name, labels to identify it and the namespace where the service will be defined.

Because was stated that we should only allow access inside our firewall (means inside our cluster) we need to create a service eith ClusterIP type:

```
spec:
  type: ClusterIP
  ports:
  - name: "redis-service"
    port: 6379
    targetPort: 6379
  selector:
    app: redis
```

Apply on Kubernetes cluster
-----

Execute the following commands and verify if everything is running properly:

```
kubectl apply -f redis-deployment.yaml
kubectl apply -f redis-service.yaml
```


