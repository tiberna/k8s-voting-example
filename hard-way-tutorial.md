Hard-way tutorial
=========

Step #1 - Postgres Database
-----

You first need to create the Database that will store the votes for the application.

The application requires Postgres version 9.4. The database should only be accessible by other microservices behind your firewall. It listens for connections on port 5432. The database stores its data in /var/lib/postgresql/data, and it needs to persist. It doesn't require network-based persistent storage, a local directory mount will do.

Finally, other microservices expect to reach this microservice with the name "db" on port 5432.

Step #2 - Redis
-----
You need to create Redis, which will collect new votes for the application.

The application requires the most recent minimal version of Redis. Redis should only be accessible by other microservices behind your firewall. It listens for connections on port 6379. Redis stores its data in /data. It doesn't require network-based persistent storage, a local directory mount will do.

Finally, other microservices expect to reach this microservice with the name "redis" on port 6379.

Step #3 - Result Microservice
-----

You need to create the Result microservice, which will present the results of the vote to end-users.

The application requires a recent version of the Result microservice, specifically dockersamples/examplevotingapp_result:before. Result should be accessible by other microservices behind your firewall. In addition, it should be accessible to end-users via port 31001.

Other microservices expect to reach the Result microservice with the name "result" via port 5001; however, the actual application listens on port 80.

Step #4 - Vote Microservice
-----

You need to create the Vote microservice, which will allow end-users to vote for their favorite pet.

The application requires a recent version of the Vote microservice, specifically dockersamples/examplevotingapp_vote:before. Vote should be accessible by other microservices behind your firewall, specifically those that are part of the Voting App. In addition, it should be accessible to end-users via port 31000.

Other microservices expect to reach the Vote microservice with the name "vote" via port 5000; however, the actual listens on port 80.

Step #5 - Worker Microservice
-----

You need to create the Worker microservice, which will receive and then store votes in the database.

The application requires a recent version of the Worker microservice, specifically dockersamples/examplevotingapp_worker. The Worker microservice does not need to be accessible to end-users.

Step #6 - Debug your app
-----

You should have a total of 5 Deployments and 4 Services at minimum. Create these objects using the Kubernetes CLI.

At this point, you should be able to access the Voting App via port 31000 and 31001. However, if it isn't working properly, you need to debug.

Check the status of all Services and Deployments. Read the logs from all of your Pods. Double-check configuration specified in all steps of the lab.

Step #7 - Interact with your Voting App
-----

Now that the application has been deployed, the developers have a few patches. The Vote microservice should be upgraded to the following version:

```
dockersamples/examplevotingapp_vote:after
```

In addition, the result microservice has an update:

```
dockersamples/examplevotingapp_result:after
```

Now that the application has been updated, you need to prepare for the launch of the application. There will be many end-users, so the frontend should be scaled up to 3 total replicas.

At this point, you've successfully deployed the Voting App, congratulations!