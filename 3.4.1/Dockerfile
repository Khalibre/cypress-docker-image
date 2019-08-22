FROM cypress/base:10

ARG CYPRESS_VERSION="3.4.1"

USER root

RUN node --version
RUN echo "force new chrome here"

# install Chromebrowser
RUN \
  wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | apt-key add - && \
  echo "deb http://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google.list && \
  apt-get update && \
  apt-get install -y dbus-x11 google-chrome-stable && \
  rm -rf /var/lib/apt/lists/*

# "fake" dbus address to prevent errors
# https://github.com/SeleniumHQ/docker-selenium/issues/87
ENV DBUS_SESSION_BUS_ADDRESS=/dev/null \
    CI=1

WORKDIR /e2e

# Add zip utility - it comes in very handy
RUN apt-get update && apt-get install -y zip && \
    chown -R node:node /e2e

# a few environment variables to make NPM installs easier
# good colors for most applications
ENV TERM xterm
# avoid million NPM install messages
ENV npm_config_loglevel warn
# allow installing when the main user is root
ENV npm_config_unsafe_perm true

# COPY /package.json /e2e/package.json
USER node

RUN npm install cypress@3.4.1 \
    mocha mochawesome \
    mochawesome-merge \
    mocha-junit-reporter \
    mocha-multi-reporters \
    mochawesome-report-generator
