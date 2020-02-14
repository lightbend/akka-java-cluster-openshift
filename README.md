# Akka-java-cluster-openshift

This is based on [Hugh's Akka Cluster](https://github.com/mckeeh3/akka-java-cluster-openshift) demo app with the nice visualizer.
We've only removed a few config
options so they go back to defaults, and updated Akka Mangement to 1.0, and then specify the app with an
AkkaCluster resource instead of Namespace + Role + Rolebinding against default account + Deployment.

The AkkaCluster resource can be installed in any namespace. This application uses the Red Hat Universal Base Image([UBI](https://www.redhat.com/en/blog/introducing-red-hat-universal-base-image])), and the [AdoptOpenJDK](https://adoptopenjdk.net/.).

## Enable Akka Management

The AkkaCluster Operator is for use with applications using [Akka Management](https://doc.akka.io/docs/akka-management/current/) v1.x or newer, with both [Bootstrap](https://doc.akka.io/docs/akka-management/current/bootstrap/index.html) and [HTTP](https://doc.akka.io/docs/akka-management/current/cluster-http-management.html) modules enabled, and a management port defined to use discovery.

```
akka.management {
  cluster.bootstrap {
    contact-point-discovery {
      discovery-method = kubernetes-api
    }
  }
}
```
## Install the AkkaCluster operator

To install the operator, use the [Akka Cluster Operator](https://operatorhub.io/operator/akka-cluster-operator) from [OperatorHub.io](https://operatorhub.io).

You can also clone the Operator repo and follow its README. It is pre-built so you're just loading
Kubernetes resources in this step and using the bintray image.

https://github.com/lightbend/akka-cluster-operator

## Build the app docker image (optional)

The public image is available but you can build your own image wtih:

```sh
mvn clean package docker:build
```
Manually publish updates to Bintray.

## Openshift

```bash
kubectl apply -f ./kubernetes/akka-cluster.yml
```

make a Route to deployment/akka-cluster-demo port 8080 for the UI

## Minikube

Install the AkkaCluster Operator from [OperatorHub.io](https://operatorhub.io/operator/akka-cluster-operator) before deploying the demo application.

```bash
kubectl apply -f ./kubernetes/akka-cluster-minishift.yml
```

Expose the built in UI:

```bash
kubectl expose deployment/akka-cluster-demo --type=NodePort --port 8080
```

Go to the UI:

```bash
minikube service akka-cluster-demo
```
