FROM ubuntu:14.04

RUN apt-get update -y
RUN apt-get install -y  build-essential wget\
                        tar unzip git xvfb firefox\
                        zlibc zlib1g zlib1g-dev bzip2 patch\
                        libgdbm3 libgdbm-dev\
                        libreadline6 libreadline6-dev\
                        libffi6 libffi-dev\
                        openssl libssl-dev\
                        gconf-service libgconf-2-4\
                        libnspr4 libnss3\
                        libpango1.0 libxss1\
                        libxtst6 libappindicator1\
                        libcurl3 xdg-utils

# Install Chrome
WORKDIR /opt
RUN wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb
RUN dpkg -i google-chrome*.deb

# Install Chrome Driver
WORKDIR /opt
RUN wget https://chromedriver.storage.googleapis.com/2.16/chromedriver_linux64.zip
RUN unzip chromedriver_linux64.zip
RUN chmod +x chromedriver
RUN mv chromedriver /usr/bin

# Install ruby
WORKDIR /opt
RUN wget --no-check-certificate https://cache.ruby-lang.org/pub/ruby/2.1/ruby-2.1.2.tar.gz &&\
    tar -xvf ruby-2.1.2.tar.gz
WORKDIR /opt/ruby-2.1.2
RUN ./configure
RUN make install
RUN gem install --no-ri --no-rdoc --user-install bundler
ENV PATH=$PATH:/root/.gem/ruby/2.1.0/bin

# Install Nodejs
WORKDIR /opt
RUN wget https://nodejs.org/dist/v0.12.7/node-v0.12.7-linux-x64.tar.gz &&\
    tar -xvf node-v0.12.7-linux-x64.tar.gz
ENV PATH=$PATH:/opt/node-v0.12.7-linux-x64/bin

RUN npm install -g phantomjs

# Set up Xvfb
ENV DISPLAY=:1.0

WORKDIR /
ADD ./run_xvfb /
CMD ./run_xvfb
