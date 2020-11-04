Kubernetes Voting Example - Step #1
=========
On first step we'll build a [Postgres](https://hub.docker.com/_/postgres/) database backed by a Docker volume.

You first need to create the Database that will store the votes for the application.

The application requires Postgres version 9.4. The database should only be accessible by other microservices behind your firewall. It listens for connections on port 5432. The database stores its data in /var/lib/postgresql/data, and it needs to persist. It doesn't require network-based persistent storage, a local directory mount will do.

Finally, other microservices expect to reach this microservice with the name "db" on port 5432.


Deployment
-----
Create a file (be aware of the identation):

```
db-deployment.yaml
```

Define metadata for deployment:
```
apiVersion: apps/v1
kind: Deployment
metadata:
  labels:
    app: db
  name: db
  namespace: vote
```
On this part you are defining deployment name, labels to identify it and the namespace where the deployment will be defined.

Then, start to define specification for your deployment:
```
spec:
  replicas: 1
  selector:
    matchLabels:
      app: db
```

On this you're stating that you want 1 replica of the pod and must have the label 'db' assinged to it.

After that, you need to specify your pod definition:

```
template:
    metadata:
      labels:
        app: db
    spec:
      containers:
      - image: postgres:9.4
        name: postgres
        env:
        - name: POSTGRES_USER
          value: postgres
        - name: POSTGRES_PASSWORD
          value: postgres
        ports:
        - containerPort: 5432
          name: postgres
```

On this code you can check how to configure environment variables on a container that allows to define Postgres username and password

The last step is to configure the volume inside 'spec' block:
```
volumes:
- name: db-data
  emptyDir: {} 
```

And define the volume mount on the container:
```
volumeMounts:
- mountPath: /var/lib/postgresql/data
  name: db-data
```

Service
-----

Create a file (be aware of the identation):

```
db-service.yaml
```

Define metadata for service:
```
apiVersion: v1
kind: Service
metadata:
  labels:
    app: db
  name: db
  namespace: vote
```
On this part you are defining service name, labels to identify it and the namespace where the service will be defined.

Because was stated that we should only allow access inside our firewall (means inside our cluster) we need to create a service eith ClusterIP type:

```
spec:
  type: ClusterIP
  ports:
  - name: "db-service"
    port: 5432
    targetPort: 5432
  selector:
    app: db
```

Apply on Kubernetes cluster
-----

Execute the following commands and verify if everything is running properly:

```
kubectl apply -f db-deployment.yaml
kubectl apply -f db-service.yaml
```


