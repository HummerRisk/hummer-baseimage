FROM ubuntu:22.04 as build-env

USER root

ENV JAVA_APP_DIR=/deployments
ENV JAVA_MAJOR_VERSION=11

# Time Zone
ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=Asia/Shanghai

# pre-requisite distro deps, and build env setup
# RUN adduser --disabled-login --gecos "" custodian
RUN sed -i s@/archive.ubuntu.com/@/mirrors.aliyun.com/@g /etc/apt/sources.list
RUN apt-get --yes clean
RUN apt-get --yes update
RUN apt-get --yes install build-essential curl unzip python3-venv python3-dev --no-install-recommends
RUN python3 -m venv /usr/local

WORKDIR /src

# instal jdk11
RUN apt-get install openjdk-11-jre  --yes  \
  && echo "securerandom.source=file:/dev/urandom" >> /usr/lib/jvm/java-11-openjdk-amd64/lib/security/java.security

# Agent bond including Jolokia and jmx_exporter
ADD agent-bond-opts /opt/run-java-options
RUN mkdir -p /opt/agent-bond \
 && curl https://repo.maven.apache.org/maven2/io/fabric8/agent-bond-agent/1.2.0/agent-bond-agent-1.2.0.jar \
          -o /opt/agent-bond/agent-bond.jar \
 && chmod 444 /opt/agent-bond/agent-bond.jar \
 && chmod 755 /opt/run-java-options
ADD jmx_exporter_config.yml /opt/agent-bond/
EXPOSE 8778 9779

# Add run script as /deployments/run-java.sh and make it executable
COPY run-java.sh /deployments/
RUN chmod 755 /deployments/run-java.sh
RUN curl https://company.hummercloud.com/offline-package/sigar/sigar.zip -o sigar.zip && unzip sigar.zip -d /usr/bin/ && rm -rf sigar.zip

CMD [ "/deployments/run-java.sh" ]
