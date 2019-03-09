Red Hat CDK Guide
===

This document describes how to use this repository with [Red Hat Container Development Kit (CDK)](https://developers.redhat.com/products/cdk).

Overview
---

Red Hat CDK is essentially a flavour of [minishift](https://github.com/minishift/minishift) suitable for OpenShift local development.

`minishift` binary is used to start a Red Hat Enterprise Linux (RHEL) VM that contains a single-node OpenShift instance.

This is particularly useful for working with Red Hat container images that require a subscribed RHEL operating system to function.

Install CDK
---

Red Hat offers a _no-cost RHEL developer subscription_ which entitles you to subscribe one RHEL system. Before starting, sign up for this subscription at [https://developers.redhat.com/products/rhel/download/]().

Once ready, download [CDK binary](https://developers.redhat.com/products/cdk/download/) for your OS and follow the [instructions](https://developers.redhat.com/products/cdk/hello-world/) to set it up.

Refer to [Getting Started Guide](https://access.redhat.com/documentation/en-us/red_hat_container_development_kit/3.7/html-single/getting_started_guide/#getting_started_with_container_development_kit) if you get stuck.

Start CDK
---

Once the CDK is successfully installed, you start an instance by using `minishift start` command.

Here is the one I use on my MacOS: 

```
minishift start --username <red hat username> --profile mule --vm-driver xhyve --openshift-version v3.11.43
```

`--profile` feature is handy if you work with a few different instances of minishift.

Tip: `minishift --help` and `minishift <sub-command> --help` are a good way of finding what options are available to you.

Use CDK
---

The [Getting Started Guide](https://access.redhat.com/documentation/en-us/red_hat_container_development_kit/3.7/html-single/getting_started_guide/#using_cdk) has an extensive section on how to use CDK but here are some tips when working with this repo.

* I tend to login as `system:admin` rather than `developer` to have unfettered access to OpenShift features during development.
* On MacOS, `eval $(minishift oc-env)` can be used to use the correct `oc` and log into the OpenShift instance.
* On MacOS, `eval $(minishift docker-env)` can be used to point  the Docker client to the Docker daemon running within the RHEL VM. This is particularly handy for doing `docker build` with Red Hat base images.