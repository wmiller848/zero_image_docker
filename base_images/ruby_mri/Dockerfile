FROM artifactory.roving.com/social/confd:latest

MAINTAINER wmiller@constantcontact.com

RUN yum -y install curl-devel zlib-devel openssl-devel pcre-devel
RUN yum -y install glibc-devel glibc-headers glibc-static gcc cpp gcc-c++
RUN yum -y install hostname make cmake autoconf automake patch
RUN yum -y install wget git tar curl which
RUN yum -y install openssl

# install ruby
WORKDIR /opt
# Issue with 2.2 and rails 3.2.22
RUN wget --no-check-certificate https://cache.ruby-lang.org/pub/ruby/2.1/ruby-2.1.2.tar.gz
RUN tar -xvf ruby-2.1.2.tar.gz
WORKDIR /opt/ruby-2.1.2
RUN ./configure
RUN make install

# install bundler
RUN gem install -V --no-ri --no-rdoc bundler

RUN echo "ruby --version" >> /run.sh
