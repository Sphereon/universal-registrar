# Dockerfile for universalregistrar/uni-registrar-web

FROM maven:3-jdk-8
MAINTAINER Markus Sabadello <markus@danubetech.com>

USER root

# build dependencies

RUN cd /opt/ && git clone https://github.com/decentralized-identity/did-common-java.git
RUN cd /opt/did-common-java/ && mvn clean install -DskipTests

RUN mvn org.apache.maven.plugins:maven-dependency-plugin:3.1.1:get -DrepoUrl=https://repo.maven.apache.org/maven2 -Dartifact=org.eclipse.jetty:jetty-maven-plugin:9.4.18.v20190429
RUN mvn org.apache.maven.plugins:maven-dependency-plugin:3.1.1:get -DrepoUrl=https://repo.maven.apache.org/maven2 -Dartifact=org.apache.maven.plugins:maven-war-plugin:3.2.2

# build uni-registrar-web

ADD ./registrar/java/ /opt/uni-registrar-java
ADD ./config.json /opt/uni-registrar-java/

RUN cd /opt/uni-registrar-java && mvn clean && \
    cd /opt/uni-registrar-java && mvn install -N -DskipTests && \
    cd /opt/uni-registrar-java/uni-registrar-core && mvn install -N -DskipTests && \
    cd /opt/uni-registrar-java/driver && mvn install -N -DskipTests && \
    cd /opt/uni-registrar-java/driver-http && mvn install -N -DskipTests && \
    cd /opt/uni-registrar-java/uni-registrar-local && mvn install -N -DskipTests && \
    cd /opt/uni-registrar-java/uni-registrar-web && mvn install -N -DskipTests

# done

EXPOSE 9080

RUN chmod a+rx /opt/uni-registrar-java/uni-registrar-web/docker/run-uni-registrar-web.sh
CMD "/opt/uni-registrar-java/docker/run-uni-registrar-web.sh"