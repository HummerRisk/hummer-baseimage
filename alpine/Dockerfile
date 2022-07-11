FROM alpine:3.11.12

USER root

ENV JAVA_APP_DIR=/deployments
ENV JAVA_MAJOR_VERSION=8

# Time Zone
ENV DEBIAN_FRONTEND=noninteractive 
ENV TZ=Asia/Shanghai

RUN mkdir -p /deployments

# JAVA_APP_DIR is used by run-java.sh for finding the binaries
ENV JAVA_APP_DIR=/deployments \
    JAVA_MAJOR_VERSION=8

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/g' /etc/apk/repositories


# /dev/urandom is used as random source, which is perfectly safe
# according to http://www.2uo.de/myths-about-urandom/
RUN apk add --update \
    curl \
    openjdk8=8.275.01-r0 \
 && rm /var/cache/apk/* \
 && echo "securerandom.source=file:/dev/urandom" >> /usr/lib/jvm/default-jvm/jre/lib/security/java.security

# install python3
RUN apk add python3=3.8.10-r0 python3-dev=3.8.10-r0 py3-configobj py3-pip py3-setuptools
RUN python3 -m venv /usr/local


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

