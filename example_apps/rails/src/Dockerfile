FROM artifactory.roving.com/social/passenger:0.0.3

MAINTAINER wmiller@constantcontact.com

RUN yum -y install sqlite-devel

ADD ./ /opt/cc/apps/rails_example

WORKDIR /opt/cc/apps/rails_example

RUN bundle install
RUN RAILS_ENV=production bundle exec rake assets:precompile

ADD ./nginx/rails.conf /etc/nginx/sites-enabled/
