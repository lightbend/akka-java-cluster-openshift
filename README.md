# akka-java-cluster-openshift

An Akka Java cluster OpenShift demo application

This is an amazing way to visualize Akka Cluster behavior, and demonstrate core reactive systems principles!

## build

```sh
mvn clean package docker:build
```

If this doesn't work, you can `git checkout -b bintray` and go straight to the deploy step, using a pre-built
image published to bintray.

## openshift

```bash
git checkout -b bintray
kubectl apply -f kubernetes
```

make a Route to deployment/akka-cluster-demo port 8080 for the UI

## minikube

Install the AkkaCluster Operator.

Build the app against minikube's docker environment.
Verify that the `akka-cluster-demo:1.0.2` image is in minikube's registry:

```bash
docker images
```

Deploy the app:

```bash
kubectl apply -f kubernetes/akka-cluster.yml
```

Expose the built in UI:

```bash
kubectl expose deployment/akka-cluster-demo --type=NodePort --port 8080
```

Go to the UI:

```bash
minikube service akka-cluster-demo
```
