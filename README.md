# akka-java-cluster-openshift

This is based on [Hugh's Akka Cluster](https://github.com/mckeeh3/akka-java-cluster-openshift) demo app with the nice visualizer.
We've only removed a few config
options so they go back to defaults, and updated Akka Mangement to 1.0, and then specify the app with an
AkkaCluster resource instead of Namespace + Role + Rolebinding against default account + Deployment.

The AkkaCluster resource can be installed in any namespace.

## install the AkkaCluster operator

To install the operator, clone this repo and follow README. It is pre-built so you're just loading
Kubernetes resources in this step and using the bintray image.

https://github.com/lightbend/akka-cluster-operator

## build the app docker image (optional)

```sh
mvn clean package docker:build
```

If this doesn't work, you can `git checkout bintray` and go straight to the deploy step, using a pre-built
image published to bintray.

## openshift

```bash
git checkout bintray
kubectl apply -f ./kubernetes
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
kubectl apply -f ./kubernetes
```

Expose the built in UI:

```bash
kubectl expose deployment/akka-cluster-demo --type=NodePort --port 8080
```

Go to the UI:

```bash
minikube service akka-cluster-demo
```
