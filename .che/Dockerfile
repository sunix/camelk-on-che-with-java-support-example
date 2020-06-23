FROM quay.io/eclipse/che-sidecar-kubernetes-tooling:1.2.0-cbde020

# Maven is required to prepopulate Camel K dependencies. it also allows a faster VS Code Camel K extension execution as it doesn't need to download it when executed.
ENV MAVEN_VERSION 3.6.3
RUN mkdir -p /usr/share/maven /usr/share/maven/ref \
  && curl -fsSL -o /tmp/apache-maven.tar.gz https://apache.osuosl.org/maven/maven-3/${MAVEN_VERSION}/binaries/apache-maven-${MAVEN_VERSION}-bin.tar.gz \
  && tar -xzf /tmp/apache-maven.tar.gz -C /usr/share/maven --strip-components=1 \
  && rm -f /tmp/apache-maven.tar.gz \
  && ln -s /usr/share/maven/bin/mvn /usr/bin/mvn
ENV MAVEN_HOME /usr/share/maven

# Install a JDK (Java Development Kit). It is required for VS Code Java. 
RUN dnf -y update && \
    dnf -y install java-11-openjdk-devel && \
    dnf clean all;

# Set kamel binary on path
ENV KAMEL_VERSION 1.0.0
RUN curl -L https://github.com/apache/camel-k/releases/download/${KAMEL_VERSION}/camel-k-client-${KAMEL_VERSION}-linux-64bit.tar.gz | tar -C /usr/local/bin -xz \
    && chmod +x /usr/local/bin/kamel

# Prepopulate Camel K dependencies folder used by VS Code Camel K and VS Code Java
ENV DEFAULT_CAMEL_VERSION_CORRESPONDING_TO_KAMEL_VERSION 3.3.0
ENV VSCODE_CAMEL_K_TAG 0.0.14
ENV VSCODE_GLOBAL_STORAGE /home/theia/home/theia
ENV DEPENDENCIES_FOLDER ${VSCODE_GLOBAL_STORAGE}/redhat.vscode-camelk/java-dependencies-${DEFAULT_CAMEL_VERSION_CORRESPONDING_TO_KAMEL_VERSION}
RUN mkdir -p ${DEPENDENCIES_FOLDER} \
  && curl https://raw.githubusercontent.com/camel-tooling/vscode-camelk/${VSCODE_CAMEL_K_TAG}/resources/maven-project/pom-to-copy-java-dependencies.xml -o pom.xml\
  && mvn dependency:copy-dependencies -DcamelVersion=${DEFAULT_CAMEL_VERSION_CORRESPONDING_TO_KAMEL_VERSION} -DoutputDirectory=${DEPENDENCIES_FOLDER} \
  && rm pom.xml

RUN for f in "/home/theia" "/etc/passwd" "/projects"; do \
  echo "Changing permissions on ${f}" && chgrp -R 0 ${f} && \
  chmod -R g+rwX ${f}; \
done

# Override entrypoint to reuse the one from che-sidecar-java
ADD etc/before-start.sh /before-start.sh
ADD etc/entrypoint.sh /entrypoint.sh
RUN ["chmod", "+x", "/entrypoint.sh"]
ENTRYPOINT [ "/entrypoint.sh" ]
CMD ${PLUGIN_REMOTE_ENDPOINT_EXECUTABLE}
