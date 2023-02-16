FROM circleci/ruby:3.0.1-node-browsers

USER root

RUN apt-get update && apt-get install -y \
    apt-transport-https \
    libssl1.1 \
  && rm -rf /var/lib/apt/lists/*

RUN ["/bin/bash", "-c", "set -o pipefail && curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -"]
RUN ["/bin/bash", "-c", "set -o pipefail && curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list | tee /etc/apt/sources.list.d/msprod.list"]
# RUN wget "http://security-cdn.debian.org/debian-security/pool/updates/main/o/openssl/libssl1.0.0_1.0.1t-1+deb8u10_amd64.deb"
# RUN apt install ./libssl1.0.0_1.0.1t-1+deb8u10_amd64.deb && rm -rf /var/lib/apt/lists/*
RUN apt-get update && ACCEPT_EULA=Y apt-get install mssql-tools
RUN mkdir /tmp/src
RUN ["/bin/bash", "-c", "set -o pipefail && curl https://www.freetds.org/files/stable/freetds-1.1.12.tar.gz | tar xvz -C /tmp/src/"]
RUN cd /tmp/src/freetds-1.1.12 && ./configure --prefix=/usr/local --with-tdsver=7.3
RUN cd /tmp/src/freetds-1.1.12 && make
RUN cd /tmp/src/freetds-1.1.12 && make install
