FROM ubuntu@sha256:e4a134999bea4abb4a27bc437e6118fdddfb172e1b9d683129b74d254af51675

MAINTAINER Latchezar Hristov

ENV HOME=/home/tms

RUN useradd --user-group --create-home --shell /bin/false tms

WORKDIR $HOME

RUN apt-get update &&\
  apt-get install -y curl git

RUN curl -sL https://deb.nodesource.com/setup_6.x | bash -

RUN apt-get -y install build-essential

RUN apt-get -y install nodejs

RUN mkdir .npm && chmod 777 .npm &&\
  npm config set cache $(pwd)/.npm --global

RUN npm install bower -g --loglevel error &&\
  npm install gulp -g --loglevel error

COPY ${BASE_PATH}/node_modules $HOME/node_modules
COPY ${BASE_PATH}/client $HOME/client
COPY ${BASE_PATH}/super $HOME/super
COPY ${BASE_PATH}/school $HOME/school
COPY ${BASE_PATH}/config $HOME/config
COPY ${BASE_PATH}/gulpfile.js ./package.json $HOME/
COPY ${BASE_PATH}/laradock/gulp-console/docker.json $HOME/config/docker.json

RUN chown -R tms:tms $HOME/*

RUN chown -R tms:tms $HOME/.npm

USER tms
RUN npm install --loglevel error

USER root
RUN cd $HOME/client && bower install --allow-root && bower update --allow-root

COPY ${BASE_PATH}/node_modules $HOME/node_modules
COPY ${BASE_PATH}/client $HOME/client
COPY ${BASE_PATH}/super $HOME/super
COPY ${BASE_PATH}/school $HOME/school
COPY ${BASE_PATH}/config $HOME/config
COPY ${BASE_PATH}/gulpfile.js ./package.json $HOME/
COPY ${BASE_PATH}/laradock/gulp-console/docker.json $HOME/config/docker.json

RUN chown -R tms:tms $HOME

# Compile all JS/CSS
# USER tms
# RUN cd $HOME; HOME=$HOME TEMP=/tmp bash -c 'gulp client && gulp super && gulp school'
