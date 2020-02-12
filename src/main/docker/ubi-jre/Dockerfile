FROM registry.access.redhat.com/ubi8/ubi:latest
LABEL maintainer="Lightbend, Inc"

### Required OpenShift Certified Labels
LABEL name="Akka Cluster Operator Demo Application" \
      vendor="Lightbend, Inc." \
      version="v1.1.0" \
      release="1.1.0" \
      summary="Sample Akka Cluster application." \
      description="Sample of [Akka Cluster](https://doc.akka.io/docs/akka/current/common/cluster.html) and the [Akka Cluster Operator](https://operatorhub.io/operator/akka-cluster-operator)."

# Certified image spec
RUN  mkdir /licenses
COPY LICENSE /licenses

# Update image
RUN yum update --disableplugin=subscription-manager -y && rm -rf /var/cache/yum

# Install OpenJDK
COPY adoptopenjdk.repo /etc/yum.repos.d/adoptopenjdk.repo
RUN yum update --disableplugin=subscription-manager -y && rm -rf /var/cache/yum
RUN yum install -y adoptopenjdk-8-hotspot-jre && yum clean all -y

# http, management, remoting
EXPOSE 8080 8558 2552

# Add Lightbend User and permit to access 
RUN useradd -r -m -u 1001 -g 0 lightbenduser
COPY maven /maven/
RUN chgrp -R 0 /maven && chmod -R g=u /maven
ENTRYPOINT ["/bin/sh","-c","java -jar /maven/akka-cluster-openshift.jar"]

# See also: https://docs.openshift.com/container-platform/3.11/creating_images/guidelines.html
USER 1001