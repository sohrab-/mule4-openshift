Mule 4 on OpenShift
===

This is the companion repository to [_Mule on OpenShift: Part 2 - Build & Deploy_](https://platform.deloitte.com.au/articles/mule-on-openshift-part-2) blog post.

Here you will find:

* `image`: An OpenShift-ready Mule 4 base image
* `sample-app`: A sample Mule 4 application to build and deploy on top of the base image

Refer to the README file in each directory for more details.

Environment
---

This repo has been tested with Red Hat OpenShift Container Platform 3.11.

You may gain access to an instance by either signing up for [OpenShift Online](https://www.openshift.com/trial/) or running minishift as part of [Red Hat CDK](https://developers.redhat.com/products/cdk) locally.

See [Red Hat CDK Guide](REDHAT_CDK.md) for tips on how to use CDK with this repo.

Caveat
---

Please note that some features, typically present in a production-ready image, have been stripped away in order to simplify the implementation for educational purposes. Examples include:

* Pre-process application archive to 
    * Remove any sensitive information
    * Align to container-friendly logging configuration
* Enable properties encryption/decryption
* Remove any JARs that score high in _Common Vulnerability Scoring System (CVSS)_
* Add any common user libraries, not shipped with the runtime
* CI/CD assets