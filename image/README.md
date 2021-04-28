Mule:Unwrapped 4 Container Image
===

Mule Runtime Archive
---

Mule runtime archive is not publicly available but is required for this image to build.

For demonstration purposes, this Dockerfile expects the archive, i.e. `mule-ee-distribution-standalone-4.3.0-20210322.zip`, to be present in this directory. Simply download the archive from MuleSoft support portal and place it here.

The best practice is for the archive to be hosted somewhere within the enterprise on a HTTP server (e.g. an artifact repository) and Dockerfile just `curl`s the archive into the image.

Docker Build
---

If you have access to Red Hat subscribed system with Docker installed, you can try building the image directly:

```shell
docker build -t mule4 .
```

If not, see the next section.

OpenShift Build
---

Assuming you have access to an OpenShift instance and are already logged in:

```shell
# Import the base image into openshift project
# Best practice to cache upstream images so we can control their proliferation and protect against upstream registry outages
oc import-image ubi8/openjdk-8 --from=registry.access.redhat.com/ubi8/openjdk-8 --confirm --all=true

# Create ImageStream and BuildConfig
oc apply -f openshift/build.yaml
# Build the image!
oc start-build buildConfig/mule4 --from-dir=. --wait --follow
```

Running Mule Applications
---

When running containerized mule appliations based on this base image, the metaspace size needs to be adjusted using explicit environment variables rather than JAVA_OPTIONS.

Remember to add these environment variables to your application deployment/deploymentconfig
```
- name: GC_MAX_METASPACE_SIZE
    value: "256"
- name: GC_METASPACE_SIZE
    value: "128"
```
