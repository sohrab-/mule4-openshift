FROM registry.access.redhat.com/ubi8/openjdk-8:1.3

ARG MULE_VERSION=4.3.0-20210322

ENV \
  MULE_HOME=/opt/mule \
  # For s2i
  DEPLOYMENTS_DIR=/deployments \
  S2I_SCRIPTS_DIR=/usr/local/s2i \
  # For java-run-sh
  JAVA_APP_NAME=mule \
  JAVA_APP_DIR=${MULE_HOME} \
  JAVA_LIB_DIR=/opt \
  JAVA_MAIN_CLASS=org.mule.runtime.module.reboot.MuleContainerBootstrap

LABEL \
  maintainer="sohrab" \
  name="mule4" \
  summary="Mule:Unwrapped 4 on UBI8 OpenJDK" \
  description="Source To Image (S2I) Mule:Unwrapped image for Red Hat OpenShift" \
  io.k8s.display-name="Mule:Unwrapped 4" \
  io.k8s.description="Source To Image (S2I) Mule:Unwrapped image for Red Hat OpenShift" \
  io.openshift.s2i.destination="${DEPLOYMENTS_DIR}" \
  io.openshift.s2i.scripts-url="image://${S2I_SCRIPTS_DIR}" \
  io.openshift.tags="mule,java"

# Elevate privileges mostly for package installation and permission update
USER root

COPY run-java-options classpath /opt/
COPY assemble /usr/local/s2i/
COPY mule-ee-distribution-standalone-${MULE_VERSION}.zip /tmp/

RUN \
  # Install Mule server
  unzip /tmp/mule-ee-distribution-standalone-${MULE_VERSION}.zip -d /opt \
	&& ln -s /opt/mule-enterprise-standalone-${MULE_VERSION} ${MULE_HOME} \
 
  # Support arbitrary user IDs: https://docs.openshift.com/container-platform/latest/creating_images/guidelines.html#openshift-container-platform-specific-guidelines
  && chown -R 185:root /opt ${S2I_SCRIPTS_DIR} ${DEPLOYMENTS_DIR} \
  && chmod -R g+rwX /opt ${S2I_SCRIPTS_DIR} ${DEPLOYMENTS_DIR} \
  && chmod +x ${S2I_SCRIPTS_DIR}/{assemble,run} \
  # Clean-up
  && rm -rvf /tmp/mule-ee-distribution-standalone-${MULE_VERSION}.zip

# OpenShift does not let you run a container as root
USER 185