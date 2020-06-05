FROM quay.io/eclipse/che-sidecar-kubernetes-tooling:1.1.0-24dc4a3

ENV MAVEN_VERSION 3.6.3
RUN mkdir -p /usr/share/maven /usr/share/maven/ref \
  && curl -fsSL -o /tmp/apache-maven.tar.gz https://apache.osuosl.org/maven/maven-3/${MAVEN_VERSION}/binaries/apache-maven-${MAVEN_VERSION}-bin.tar.gz \
  && tar -xzf /tmp/apache-maven.tar.gz -C /usr/share/maven --strip-components=1 \
  && rm -f /tmp/apache-maven.tar.gz \
  && ln -s /usr/share/maven/bin/mvn /usr/bin/mvn
ENV MAVEN_HOME /usr/share/maven

RUN dnf -y update && \
    dnf -y install java-11-openjdk && \
    dnf clean all;

ENV KAMEL_VERSION 1.0.0-RC2
RUN curl -L https://github.com/apache/camel-k/releases/download/${KAMEL_VERSION}/camel-k-client-${KAMEL_VERSION}-linux-64bit.tar.gz | tar -C /usr/local/bin -xz \
    && chmod +x /usr/local/bin/kamel

# Prepopulate Camel K dependencies folder used by VS Code Camel K and VS Code Java
ENV DEFAULT_CAMEL_VERSION_CORRESPONDING_TO_KAMEL_VERSION 3.0.1
ENV VSCODE_CAMEL_K_TAG 0.0.13
ENV DEPENDENCIES_FOLDER /home/theia/home/theia/redhat.vscode-camelk/java-dependencies-${DEFAULT_CAMEL_VERSION_CORRESPONDING_TO_KAMEL_VERSION}
RUN mkdir -p ${DEPENDENCIES_FOLDER} \
  && curl https://raw.githubusercontent.com/camel-tooling/vscode-camelk/${VSCODE_CAMEL_K_TAG}/resources/maven-project/pom-to-copy-java-dependencies.xml -o pom.xml\
  && mvn dependency:copy-dependencies -DcamelVersion=${DEFAULT_CAMEL_VERSION_CORRESPONDING_TO_KAMEL_VERSION} -DoutputDirectory=${DEPENDENCIES_FOLDER} \
  && rm pom.xml
