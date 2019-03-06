Mule 4 on OpenShift
===

This is the companion repository to [_Mule on OpenShift: Part 2 - Build & Deploy_](https://platform.deloitte.com.au/articles/mule-on-openshift-part-2) blog post.

Here you will find:

* `image`: An OpenShift-ready Mule 4 base image
* `sample-app`: A sample Mule 4 application to build and deploy on top of the base image

Refer to the README file in each repo for more details.

Environment
---

This repo has been tested with Red Hat OpenShift Container Platform 3.11.

You may gain access to an instance by either signing up for [OpenShift Online](https://www.openshift.com/trial/) or running minishift as part of [Red Hat CDK](https://developers.redhat.com/products/cdk) locally.

Git Sub-module
---

This repository brings in [`fabric8io-images/java`](https://github.com/fabric8io-images/java) repository (tag `1.5.4`) as a git sub-module at `image/fabric8-java-images`.

Ensure this repo is checked out correctly as part of your clone before attempting any builds.

Caveat
---

Please note that some features, usually present in production-ready images, have been stripped away in order to simplify the implementation for educational purposes. Examples include:

* Pre-process application archive to 
    * Remove any sensitive information
    * Align to container-friendly logging configuration
* Enable properties encryption/decryption
* Remove any JARs that score high in _Common Vulnerability Scoring System (CVSS)_
* Add any common user libraries, not shipped with the runtime