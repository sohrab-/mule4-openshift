FROM registry.access.redhat.com/rhel-minimal:7.6

ARG JAVA_VERSION=1.8.0
ARG MULE_VERSION=4.2.0
ARG AGENT_BOND_VERSION=1.2.0

ENV \
  JAVA_MAJOR_VERSION=8 \
  JAVA_HOME=/usr/lib/jvm/jre \
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
  summary="Mule:Unwrapped 4 on mininmal RHEL" \
  description="Source To Image (S2I) Mule:Unwrapped image for Red Hat OpenShift" \
  io.k8s.display-name="Mule:Unwrapped 4" \
  io.k8s.description="Source To Image (S2I) Mule:Unwrapped image for Red Hat OpenShift" \
  io.openshift.s2i.destination="${DEPLOYMENTS_DIR}" \
  io.openshift.s2i.scripts-url="image://${S2I_SCRIPTS_DIR}" \
  io.openshift.tags="mule,java"

# Jolokia and JMX exporter ports
EXPOSE 8778 9779
# Superceded by the s2i run script
CMD [ "/deployments/run-java.sh" ]

# Elevate privileges mostly for package installation and permission update
USER root

COPY fabric8-java-images/images/jboss/openjdk8/jdk/agent-bond-opts /opt/agent-bond-opts
COPY fabric8-java-images/images/jboss/openjdk8/jdk/jmx_exporter_config.yml /opt/agent-bond/
COPY fabric8-java-images/images/jboss/openjdk8/jdk/run-java.sh ${DEPLOYMENTS_DIR}/
COPY run-java-options classpath /opt/
COPY assemble run /usr/local/s2i/

# Mule rutime archive is not publicly available. Download it from Mule support site.
# Ideally it should be hosted somewhere else and curl'ed into the image in the subsequent RUN
COPY mule-ee-distribution-standalone-${MULE_VERSION}.tar.gz /tmp/

RUN \
  # Install supporting tools
  microdnf --enablerepo=rhel-7-server-rpms --nodocs install \
    gzip \
    java-${JAVA_VERSION}-openjdk-headless \
    tar \
    unzip \
  # Install Mule server
  # && curl --fail --silent --show-error \
	# 	https://<some artifact repository>/mule-ee-distribution-standalone-${MULE_VERSION}.tar.gz \
	# 	> /tmp/mule-ee-distribution-standalone-${MULE_VERSION}.tar.gz \
  && tar -xvf /tmp/mule-ee-distribution-standalone-${MULE_VERSION}.tar.gz --directory /opt \
	&& ln -s /opt/mule-enterprise-standalone-${MULE_VERSION} ${MULE_HOME} \
  # Install agent-bond
  && curl --fail --silent --show-error \
    http://central.maven.org/maven2/io/fabric8/agent-bond-agent/${AGENT_BOND_VERSION}/agent-bond-agent-${AGENT_BOND_VERSION}.jar \
    > /opt/agent-bond/agent-bond.jar \
  # Support arbitrary user IDs: https://docs.openshift.com/container-platform/latest/creating_images/guidelines.html#openshift-container-platform-specific-guidelines
  && chown -R 1001:root /opt ${S2I_SCRIPTS_DIR} ${DEPLOYMENTS_DIR} \
  && chmod -R g+rwX /opt ${S2I_SCRIPTS_DIR} ${DEPLOYMENTS_DIR} \
  && chmod +x ${DEPLOYMENTS_DIR}/run-java.sh ${S2I_SCRIPTS_DIR}/{assemble,run} \
  # Clean-up
  && rm -rvf /tmp/mule-ee-distribution-standalone-${MULE_VERSION}.tar.gz \
  && microdnf clean all

# OpenShift does not let you run a container as root
USER 1001