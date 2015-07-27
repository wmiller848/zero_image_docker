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
  * Untar the app.tar.gz
  * Run the docker-compose file you want (f1.yml, l1.yml, etc..)
