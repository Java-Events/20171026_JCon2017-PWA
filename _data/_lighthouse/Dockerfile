FROM ubuntu:latest
MAINTAINER Sven Ruppert <sven.ruppert@gmail.com>

USER root
WORKDIR /app


RUN apt-get -y update && \
  apt-get -y install --no-install-recommends -y curl chromium-browser software-properties-common && \
  rm -fr /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN curl -sL https://github.com/shyiko/jabba/raw/master/install.sh | \
    JABBA_COMMAND="install adopt@1.8.162-00 -o /jdk" bash

RUN curl -sL https://deb.nodesource.com/setup_8.x | bash -
RUN apt-get install --no-install-recommends -y nodejs && \
    rm -fr /var/lib/apt/lists/* /tmp/* /var/tmp/*
RUN npm install -g lighthouse

ENV JAVA_HOME /jdk
ENV PATH $JAVA_HOME/bin:$PATH
RUN useradd -ms /bin/bash  -g root lighthouse
USER lighthouse

RUN mkdir /home/lighthouse/report
WORKDIR /home/lighthouse/report

ENTRYPOINT nohup java -jar fat.jar & \
           lighthouse --chrome-flags="--headless --no-sandbox" ${lighthouse_url}

#           RUN apt-get install -y xvfb
#           ENV DISPLAY=1.5
#           ENV TMP_PROFILE_DIR=$(mktemp -d -t lighthouse.XXXXXXXXXX)
#           RUN mkdir /tmp/lighthouse
#           RUN chmod 777 /tmp/lighthouse
#           ENV TMP_PROFILE_DIR=/tmp/lighthouse
#
#           ENTRYPOINT xvfb-run --server-args='-screen 0, 1024x768x16' \
#                          chromium-browser --user-data-dir=$TMP_PROFILE_DIR \
#                          --start-maximized \
#                          --disable-namespace-sandbox \
#                          --no-sandbox \
#                          --no-first-run \
#                          --remote-debugging-port=9222 "about:blank" && \
#                      lighthouse --port=9222 https://github.com
#docker system prune --all
#docker volume prune
#docker system df