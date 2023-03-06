FROM cimg/ruby:3.1.0-browsers

USER root

ENV RUBY_VERSION=3.1.0 \
	  RUBY_MAJOR=3.1

RUN apt-get update && apt-get install -y \
    apt-transport-https \
    libssl1.1 \
  && rm -rf /var/lib/apt/lists/* \
  echo "gem: --no-document" > ~/.gemrc && \
	mkdir -p ~/ruby && \
	downloadURL="https://cache.ruby-lang.org/pub/ruby/${RUBY_MAJOR}/ruby-$RUBY_VERSION.tar.gz" && \
	curl -sSL $downloadURL | tar -xz -C ~/ruby --strip-components=1 && \
	cd ~/ruby && \
	autoconf && \
	./configure --enable-shared && \
	make -j "$(nproc)" && \
	sudo make install && \
	mkdir ~/.rubygems && \
	sudo rm -rf ~/ruby /var/lib/apt/lists/* && \
	cd && \
	ruby --version && \
	gem --version && \
	sudo gem update --system && \
	gem --version && \
	bundle --version

ENV GEM_HOME /home/circleci/.rubygems

RUN ["/bin/bash", "-c", "set -o pipefail && curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -"]
RUN ["/bin/bash", "-c", "set -o pipefail && curl https://packages.microsoft.com/config/ubuntu/20.04/prod.list | tee /etc/apt/sources.list.d/msprod.list"]
# RUN wget "http://security-cdn.debian.org/debian-security/pool/updates/main/o/openssl/libssl1.0.0_1.0.1t-1+deb8u10_amd64.deb"
# RUN apt install ./libssl1.0.0_1.0.1t-1+deb8u10_amd64.deb && rm -rf /var/lib/apt/lists/*
RUN apt-get update && ACCEPT_EULA=Y apt-get install mssql-tools
RUN mkdir /tmp/src
RUN ["/bin/bash", "-c", "set -o pipefail && curl https://www.freetds.org/files/stable/freetds-1.3.6.tar.gz | tar xvz -C /tmp/src/"]
RUN cd /tmp/src/freetds-1.3.6 && ./configure --with-openssl=/usr/include/openssl --enable-msdblib --with-tdsver=7.4
RUN cd /tmp/src/freetds-1.3.6 && make
RUN cd /tmp/src/freetds-1.3.6 && make install