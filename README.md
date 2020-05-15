# Apache Camel K example with Java development support on Che

## Context

It is already possible to leverage Java support for standalone Camel K files in Che. It requires a specific devfile with modified plugin. This repository aims to showcase one possible way to leverage it [awaiting for built-in availability](https://github.com/eclipse/che/issues/16018).

## Requirements

Example to have java support on Che 7.11+ (awaiting for a built-in version).

## Get started

### Start the workspace

Start the workspace: [![Contribute](https://che.openshift.io/factory/resources/factory-contribute.svg)](https://che.openshift.io/factory?url=https://github.com/apupier/camelk-on-che-with-java-support-example/raw/master/.che/devfile.yaml) to open on che.openshift.io.

You can use your own Che instance using this url:

```
https://<your-che-instance>/factory?url=https://github.com/apupier/camelk-on-che-with-java-support-example/raw/master/.che/devfile.yaml
```

This has the advantage that you can install the Camel K instance on it, and so leverage also the deployment functionalities more easily.

### Enjoy

Open the java file and enjoy the Java completion. You can still enjoy the other existing features, see this [blog post](https://developers.redhat.com/blog/2020/01/24/apache-camel-k-development-inside-eclipse-che-iteration-1/).
