FROM ubuntu:22.04 as build-env

# Time Zone
ENV DEBIAN_FRONTEND=noninteractive
ENV TZ=Asia/Shanghai

# pre-requisite distro deps, and build env setup
# RUN adduser --disabled-login --gecos "" custodian
RUN sed -i s@/archive.ubuntu.com/@/mirrors.aliyun.com/@g /etc/apt/sources.list && \
    apt-get --yes clean && \
    apt-get --yes update && \
    apt-get --yes install build-essential git curl jq python3-venv python3-dev --no-install-recommends  && \
    python3 -m venv /usr/local