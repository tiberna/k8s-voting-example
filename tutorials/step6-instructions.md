Kubernetes Voting Example - Step #6
=========
Now let's make some changes on our solution in order to fix the issues and create a more secure configuration

### Fix Results

Regarding not having updates on results page we need to update the image from worker and results.

Change the file worker-deployment.yaml to update the image:
```
- image: tasb/example-voting-app_worker
```

Then apply the changes to the cluster 

```
kubectl apply -f worker-deployment.yaml
```

Change the file results-deployment.yaml to update the image:
```
- image: tasb/example-voting-app_result:after
```

Then apply the changes to the cluster 

```
kubectl apply -f results-deployment.yaml
```

Now try to validate your solution using the web browser and checking the logs on your pods

### Be prepared for launching

Now that the application has been updated, you need to prepare for the launch of the application. There will be many end-users, so the frontend should be scaled up to 3 total replicas.

Change results-deployment.yaml and vote-deployment.yaml to have 3 replicas of each pod running.

Then apply the changes to the cluster 

```
kubectl apply -f results-deployment.yaml
kubectl apply -f vote-deployment.yaml
```

Now check that you have now 3 replicas of each pod.



### Use secrets

Regarding Postgres configuration the password must be set using a secret do not share your password inside your repo.

Deployment
-----
Create a file (be aware of the identation):

```
secret.yaml
```

Define the secret object:
```
apiVersion: v1
kind: Secret
metadata:
  name: db-secret
  namespace: vote
data:
  username: cG9zdGdyZXM=
  password: cG9zdGdyZXM=
```
Note that the values for username and password are on Base64 notation.

To create the object on the cluster run the following command:
```
kubectl apply -f secret.yaml
```

Check that your secret was created:
```
kubectl get service -n vote
```

Then, let's change the db-deployment.yaml file to use the secret instead of clear text for database username and password:
```
env:
- name: POSTGRES_USER
    valueFrom:
    secretKeyRef:
        name: db-secret
        key: username
- name: POSTGRES_PASSWORD
    valueFrom:
    secretKeyRef:
        name: db-secret
        key: password
```

Apply the changes:
```
kubectl apply -f db-deployment.yaml
```

And list your pods and check that a new pod is created and the old one is removed. Since we're using volumes we don't lose the data

Congrats! You reach the end of this tutorial and have a Voting App running on your cluster!