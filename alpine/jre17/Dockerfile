FROM alpine:3.17.2

USER root

# Time Zone
# JAVA_APP_DIR is used by run-java.sh for finding the binaries
ENV DEBIAN_FRONTEND=noninteractive \
    TZ=Asia/Shanghai \
    LANG=C.UTF-8\
    LOG4J_FORMAT_MSG_NO_LOOKUPS=true\
    JAVA_APP_DIR=/deployments \
    JAVA_MAJOR_VERSION=17
RUN mkdir -p /deployments 

RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/g' /etc/apk/repositories

# Add run script as /deployments/run-java.sh and make it executable
COPY run-java.sh /deployments/

RUN apk update && apk add --no-cache --update \
    curl \
    openjdk17-jre \
    && rm /var/cache/apk/* \
    && echo "securerandom.source=file:/dev/urandom" >> /usr/lib/jvm/default-jvm/jre/lib/security/java.security \
    && chmod 755 /deployments/run-java.sh

CMD [ "/deployments/run-java.sh" ]

