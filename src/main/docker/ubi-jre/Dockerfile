FROM registry.access.redhat.com/ubi8/ubi:latest
LABEL maintainer="Lightbend, Inc"

# Certified image
RUN  mkdir /licenses
COPY LICENSE /licenses

# Update image
RUN yum update --disableplugin=subscription-manager -y && rm -rf /var/cache/yum

# Install OpenJDK
COPY adoptopenjdk.repo /etc/yum.repos.d/adoptopenjdk.repo
RUN yum update --disableplugin=subscription-manager -y && rm -rf /var/cache/yum
RUN yum install -y adoptopenjdk-8-hotspot-jre && yum clean all -y

EXPOSE 8080 8558 2552

# Add Lightbend User and permit to access 
RUN useradd -r -m -u 1001 -g 0 lightbenduser
COPY maven /maven/
RUN chgrp -R 0 /maven && chmod -R g=u /maven
ENTRYPOINT ["/bin/sh","-c","java -jar /maven/akka-cluster-openshift.jar"]

USER 1001