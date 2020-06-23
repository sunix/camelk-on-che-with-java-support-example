# Upstream

Follow https://github.com/eclipse/che/issues/16018 to see built-in feature progress

# Current architecture

## Specific extension

A specific extension for VS Code Camel K is used. It contains all dependencies: VS Code Kubernetes,VS Code Yaml and VS Code Java.

## Dockerfile content

It requires a specific Dockerfile. It is a combination of the sidecars for Kubernetes, Camel K and Java plugins. It has been pushed to [apupier/che-sidecar-camelk:0.0.13-withmaven](https://hub.docker.com/layers/apupier/che-sidecar-camelk/0.0.13-withmaven/images/sha256-31f0b3bf096fa2de5d30630d4d87d6bd574f4509f2737794ccb008c3ea3b2398?context=explore)

The idea is to provide Maven and java. it is used by VS Code Camel K extension to download Camel K dependencies.

## Limitations

The fact to have all depdencies in the same sidecar is causing a maintenance hell. That's why, this is provided as an example for advanced Devfile writers for Camel K applications.
Also, it means that there are good chance that enabling through UI another extension will not work if it requires of the included extension.

# Dockerfile publish

- `docker build . -t apupier/che-sidecar-camelk-with-java:0.0.14`
- `docker push apupier/che-sidecar-camelk-with-java:0.0.14`

Help for automation using Github Actions would be nice.


