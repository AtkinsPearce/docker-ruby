FROM cimg/ruby:3.1.0-browsers

# install static version of google chrome for fallback use
RUN wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add - && \
  sudo sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google.list' && \
  sudo apt-get update && \
  sudo apt install google-chrome-stable
RUN echo `google-chrome --version`

# Install static version of firefox for fallback use
RUN echo 'Package: *' | sudo tee -a /etc/apt/preferences.d/firefox.pref && \
	echo 'Pin: release o=LP-PPA-mozillateam' | sudo tee -a /etc/apt/preferences.d/firefox.pref && \
	echo 'Pin-Priority: 1001' | sudo tee -a /etc/apt/preferences.d/firefox.pref && \
	sudo add-apt-repository --yes ppa:mozillateam/ppa && \
	sudo apt-get install --no-install-recommends --yes firefox && \
	sudo rm -rf /var/lib/apt/lists/* && \
	firefox --version

# install mssql-tools for database setup and connection verification
RUN ["/bin/bash", "-c", "set -o pipefail && curl https://packages.microsoft.com/keys/microsoft.asc | sudo apt-key add -"]
RUN ["/bin/bash", "-c", "set -o pipefail && curl https://packages.microsoft.com/config/ubuntu/20.04/prod.list | sudo tee /etc/apt/sources.list.d/msprod.list"]
# RUN wget "http://security-cdn.debian.org/debian-security/pool/updates/main/o/openssl/libssl1.0.0_1.0.1t-1+deb8u10_amd64.deb"
# RUN apt install ./libssl1.0.0_1.0.1t-1+deb8u10_amd64.deb && rm -rf /var/lib/apt/lists/*
RUN sudo apt-get update && \
    sudo ACCEPT_EULA=Y apt-get install mssql-tools
RUN export PATH=${PATH}:/opt/mssql-tools/bin

# install freetds for tinytds native dependency
RUN mkdir /tmp/src
RUN ["/bin/bash", "-c", "set -o pipefail && curl https://www.freetds.org/files/stable/freetds-1.3.6.tar.gz | tar xvz -C /tmp/src/"]
RUN cd /tmp/src/freetds-1.3.6 && ./configure --with-openssl=/usr/include/openssl --enable-msdblib --with-tdsver=7.4
RUN cd /tmp/src/freetds-1.3.6 && make
RUN cd /tmp/src/freetds-1.3.6 && sudo make install

# set the locale to UTF-8 and gen the locale
RUN echo "en_US.UTF-8 UTF-8" | sudo tee /etc/locale.gen
RUN sudo locale-gen

# create github user to match GitHub Actions user ids
RUN sudo groupadd --gid=1002 github && \
	sudo useradd --uid=1001 --gid=github --create-home github && \
	echo 'github ALL=NOPASSWD: ALL' | sudo tee -a /etc/sudoers.d/51-github