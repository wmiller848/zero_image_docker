FROM artifactory.roving.com/social/ruby_mri:latest

MAINTAINER wmiller@constantcontact.com

# install nodejs
WORKDIR /opt
RUN wget https://nodejs.org/dist/v0.12.5/node-v0.12.5.tar.gz
RUN tar -xvf node-v0.12.5.tar.gz
WORKDIR /opt/node-v0.12.5
RUN ./configure
RUN make install

# install passenger gem
RUN gem install -V --no-ri --no-rdoc passenger -v 5.0.10

# build nginx with passenger module
WORKDIR /opt
RUN mkdir -p /opt/cc/logs/http
# no secure https endpoint for source
RUN wget http://nginx.org/download/nginx-1.8.0.tar.gz
RUN tar -xvf nginx-1.8.0.tar.gz
WORKDIR /opt/nginx-1.8.0
RUN ./configure --prefix=/usr/local \
  --sbin-path=/usr/local/sbin \
  --conf-path=/etc/nginx/nginx.conf \
  --error-log-path=/opt/cc/logs/http/error.log \
  --http-log-path=/opt/cc/logs/http/access.log \
  --with-http_ssl_module --with-http_addition_module \
  --with-http_gzip_static_module --with-http_spdy_module \
  --add-module=/usr/local/lib/ruby/gems/2.1.0/gems/passenger-5.0.10/ext/nginx
RUN make install

ADD ./nginx/nginx.conf /etc/nginx/nginx.conf
RUN mkdir -p /etc/nginx/sites-enabled/

EXPOSE 80
EXPOSE 443
EXPOSE 8080

RUN echo "cd /opt/cc/app && bundle install"
RUN echo "nginx" >> /run.sh
