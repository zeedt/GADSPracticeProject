# Google Cloud Fundamentals: Getting Started with GKE

### Enable necessary APIs
To use Google Kubernetes Engine on GCP, the following APIs must be enabled

* Kubernetes Engine API
* Container Registry API

If any of the APIs is missing, click Enable APIs and Services at the top. Search for the above APIs by name and enable each for your current project. (You noted the name of your GCP project above.)

### Start a Kubernetes Engine cluster
Lauch cloud shell on GCP, then set a default zone. This default zone can be stored in an environemnt variable as shown below:
```sh
$ export MY_ZONE=us-central1-a
```
In this case, the default zone set is **us-central1-a**

Let's start a Kubernetes cluster managed by Kubernetes Engine. Name the cluster webfrontend and configure it to run 2 nodes:

```sh
$ gcloud container clusters create webfrontend --zone $MY_ZONE --num-nodes 2
```

It will take a while for the cluster to be created due to the fact that it would create virtual machine instances.

After the cluster has been created, you can check the version if the cluster using the command below:

```sh
$ kubectl version
```

You can then View the running nodes in the GCP Console. On the Navigation menu, click Compute Engine > VM Instances.

### Run and deploy a container
Let's launch a single of nginx container. The following command can be used to launch a single instance of nginx:

```sh
$ kubectl create deploy nginx --image=nginx:1.17.10
```
Note : The above command would launch the default number of pods, which is 1.

The pods can be viewed using the command below:

```sh
$ kubectl get pods
```

The nginx container can be exposed to the internet using the following command:

```sh
$ kubectl expose deployment nginx --port 80 --type LoadBalancer
```

When the above command was executed, Kubernetes created a service and an external load balancer with a public IP address attached to it. The IP address remains the same for the life of the service. Any network traffic to that public IP address is routed to pods behind the service: in this case, the nginx pod.

The created service can be created using the command below:

```sh
$ kubectl get services
```
The displayed external IP address can be used to test and contact the nginx container remotely.
It may take a few seconds before the External-IP field is populated for the created service. Just re-run the **kubectl get services** command every few seconds until the field is populated.

Paste the cluster's external IP address into the address bar of the browser (new tab). The default home page of the Nginx browser is displayed.

The created service can be scaled up by executing the command below

```sh
$ kubectl scale deployment nginx --replicas 3
```
The above command scale up the services to 3

You can confirm the services has been scaled to 3 pods using the following command 

```sh
$ kubectl get pods
```
Also, confirm the external IP address of the service is still the same using the command below

```sh
$ kubectl get services
```
