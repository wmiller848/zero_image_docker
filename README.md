ctct_docker
===========

##### Requires docker & docker-compose #####

### Build base images ###
From ./
```
BASE_VERSION=0.0.3 ./boostrap
```
### Build a version of the example app ###
From ./example_apps/rails
```
VERSION=0.0.3 ./build
```

### Run the example app ###
From ./example_apps/rails_deploy
```
TARGET=../rails/app.tar.gz ENV=f1 ./deploy
```
