FROM registry.access.redhat.com/ubi8/ubi
LABEL maintainer="Lightbend, Inc"

# Update image
RUN yum update --disableplugin=subscription-manager -y && rm -rf /var/cache/yum

# Install OpenJDK 
COPY adoptopenjdk.repo /etc/yum.repos.d/adoptopenjdk.repo
RUN yum update --disableplugin=subscription-manager -y && rm -rf /var/cache/yum 
RUN yum install -y adoptopenjdk-8-hotspot-jre

# Add Lightbend User
RUN useradd -r -m -u 1001 -g 0 lightbenduser
