Mule:Unwrapped 4 Container Image
===

See [_Mule on OpenShift: Part 1 - Deployment Models_](https://platform.deloitte.com.au/articles/mule-on-openshift-part-1) blog post on what _Mule:Unwrapped_ is.

Mule Runtime Archive
---

Mule runtime archive is not publicly available but is required for this image to build.

For demonstration purposes, this Dockerfile expects the archive, i.e. `mule-ee-distribution-standalone-4.1.5.tar.gz`, to be present in this directory. Simply download the archive from MuleSoft support portal and place it here.

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
oc import-image openshift/rhel-minimal:7.6 --from=registry.access.redhat.com/rhel-minimal:7.6 --confirm -n openshift

# Create ImageStream and BuildConfig
oc apply -f openshift/build.yaml
# Build the image!
oc start-build buildConfig/mule4 --from-dir=. --wait --follow
```