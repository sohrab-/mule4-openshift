Sample Mule 4 Application
===

A painfully simple Mule 4 application to test the Mule 4 base image.

Build
---

Assuming the (Mule 4 base image)[../image] is already built:

```shell
# Create application archive
mvn clean package

# Create ImageStream and BuildConfig
oc apply -f openshift/build.yaml
# Build the image!
oc start-build buildConfig/sample-app --from-file=./target/sample-app-1.0.0-SNAPSHOT-mule-application.jar --wait --follow
```

Deploy
---

Assuming the application image is already built:

```shell
# Create DeploymentConfig and friends
oc apply -f openshift/deploy.yaml
# Rollout deployment!
oc rollout latest deploymentConfig/sample-app
```