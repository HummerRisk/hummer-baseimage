FROM alpine:3.12.12

USER root

# Time Zone
ENV DEBIAN_FRONTEND=noninteractive 
ENV TZ=Asia/Shanghai
ENV LANG=C.UTF-8  
RUN mkdir -p /deployments

# JAVA_APP_DIR is used by run-java.sh for finding the binaries
ENV JAVA_APP_DIR=/deployments
ENV JAVA_MAJOR_VERSION=8


RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.ustc.edu.cn/g' /etc/apk/repositories


# /dev/urandom is used as random source, which is perfectly safe
# according to http://www.2uo.de/myths-about-urandom/
RUN apk add --update \
    curl \
    openjdk8-jre=8.275.01-r0 \
 && rm /var/cache/apk/* \
 && echo "securerandom.source=file:/dev/urandom" >> /usr/lib/jvm/default-jvm/jre/lib/security/java.security

# install python3
# ENV PYTHON_VERSION=3.8.10 
# ENV PYTHON_PIP_VERSION=20.2.4 
# ENV GPG_KEY=0D96DF4D4110E5C43FBFB17F2D347EA6AA65421D
# ENV PYTHON_GET_PIP_SHA256=6e0bb0a2c2533361d7f297ed547237caf1b7507f197835974c0dd7eba998c53c
# ENV PYTHON_GET_PIP_URL=https://bootstrap.pypa.io/pip/get-pip.py

# RUN  /bin/sh -c set -ex  && apk add --no-cache --virtual .fetch-deps   gnupg   tar   xz   && wget -O python.tar.xz "https://www.python.org/ftp/python/${PYTHON_VERSION%%[a-z]*}/Python-$PYTHON_VERSION.tar.xz"
# RUN  wget -O python.tar.xz.asc "https://www.python.org/ftp/python/${PYTHON_VERSION%%[a-z]*}/Python-$PYTHON_VERSION.tar.xz.asc"  && export GNUPGHOME="$(mktemp -d)"  && gpg --batch --keyserver ha.pool.sks-keyservers.net --recv-keys "$GPG_KEY" 
# RUN  gpg --batch --verify python.tar.xz.asc python.tar.xz  && { command -v gpgconf > /dev/null && gpgconf --kill all || :; }  && rm -rf "$GNUPGHOME" python.tar.xz.asc  && mkdir -p /usr/src/python  && tar -xJC /usr/src/python --strip-components=1 -f python.tar.xz  
# RUN rm python.tar.xz   && apk add --no-cache --virtual .build-deps    bluez-dev   bzip2-dev   coreutils   dpkg-dev dpkg   expat-dev   findutils   gcc   gdbm-dev   libc-dev   libffi-dev   libnsl-dev   libtirpc-dev   linux-headers   make   ncurses-dev   openssl-dev   pax-utils   readline-dev   sqlite-dev   tcl-dev   tk   tk-dev   util-linux-dev   xz-dev   zlib-dev  && apk del --no-network .fetch-deps   && cd /usr/src/python  && gnuArch="$(dpkg-architecture --query DEB_BUILD_GNU_TYPE)"  && ./configure   --build="$gnuArch"   --enable-loadable-sqlite-extensions   --enable-optimizations   --enable-option-checking=fatal   --enable-shared   --with-system-expat   --with-system-ffi   --without-ensurepip  && make -j "$(nproc)"   EXTRA_CFLAGS="-DTHREAD_STACK_SIZE=0x100000"   LDFLAGS="-Wl,--strip-all"   PROFILE_TASK='-m test.regrtest --pgo    test_array    test_base64    test_binascii    test_binhex    test_binop    test_bytes    test_c_locale_coercion    test_class    test_cmath    test_codecs    test_compile    test_complex    test_csv    test_decimal    test_dict    test_float    test_fstring    test_hashlib    test_io    test_iter    test_json    test_long    test_math    test_memoryview    test_pickle    test_re    test_set    test_slice    test_struct    test_threading    test_time    test_traceback    test_unicode   '  && make install  && rm -rf /usr/src/python   && find /usr/local -depth   \(    \( -type d -a \( -name test -o -name tests -o -name idle_test \) \)    -o \( -type f -a \( -name '*.pyc' -o -name '*.pyo' -o -name '*.a' \) \)    -o \( -type f -a -name 'wininst-*.exe' \)   \) -exec rm -rf '{}' +   && find /usr/local -type f -executable -not \( -name '*tkinter*' \) -exec scanelf --needed --nobanner --format '%n#p' '{}' ';'   | tr ',' '\n'   | sort -u   | awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }'   | xargs -rt apk add --no-cache --virtual .python-rundeps  && apk del --no-network .build-deps   && python3 --version
# RUN cd /usr/local/bin  && ln -s idle3 idle  && ln -s pydoc3 pydoc  && ln -s python3 python  && ln -s python3-config python-config
# RUN /bin/sh -c set -ex;   wget -O get-pip.py "$PYTHON_GET_PIP_URL";  echo "$PYTHON_GET_PIP_SHA256 *get-pip.py" | sha256sum -c -;   python get-pip.py   --disable-pip-version-check   --no-cache-dir   "pip==$PYTHON_PIP_VERSION"  ;  pip --version;   find /usr/local -depth   \(    \( -type d -a \( -name test -o -name tests -o -name idle_test \) \)    -o    \( -type f -a \( -name '*.pyc' -o -name '*.pyo' \) \)   \) -exec rm -rf '{}' +;  rm -f get-pip.py                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        

RUN apk add --no-cache ca-certificates 
RUN apk add python3=3.8.10-r0 python3-dev=3.8.10-r0 py3-configobj py3-pip py3-setuptools  jq file coreutils 
RUN apk add --no-cache zlib-dev bzip2-dev pcre-dev openssl-dev ncurses-dev sqlite-dev readline-dev tk-dev
RUN apk add --no-cache build-base gcc g++ make cmake
RUN pip3 install --upgrade pip
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

