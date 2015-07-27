FROM artifactory.roving.com/social/rails:0.0.3

MAINTAINER wmiller@constantcontact.com

ADD ./ /opt/cc/apps/rails_example
WORKDIR /opt/cc/apps/rails_example

RUN bundle install
RUN RAILS_ENV=production bundle exec rake assets:precompile

#./nginx/rails.conf /etc/nginx/sites-enabled/
