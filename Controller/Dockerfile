# This default may be swapped for any compatible base image
ARG BASE_IMAGE=amd64/ubuntu:20.04

# This buildpack is used for tool intallation and user/directory setup
FROM containerbase/buildpack:1.6.0@sha256:ae411ab0b74176c3db1d9213cd5d40e57b03b77d56375702ee05379a437ac742 AS buildpack

FROM ${BASE_IMAGE} as base

ARG USER_NAME=user
ARG USER_ID=1542
ARG APP_ROOT=/usr/src/app

# Set env and shell
ENV BASH_ENV=/usr/local/etc/env
SHELL ["/bin/bash" , "-c"]

# Set up buildpack
COPY --from=buildpack /usr/local/bin/ /usr/local/bin/
COPY --from=buildpack /usr/local/buildpack/ /usr/local/buildpack/
RUN install-buildpack

# renovate: datasource=docker depName=openjdk versioning=docker
ARG JAVA_VERSION=8
RUN install-tool java

# copy folder
COPY docker-image/ /

# Expose ports
# HTTP
EXPOSE 5678

# home directory
ENV HOME=/etc/usr/local/whitesource
ENV WHITESOURCE_HOME=$HOME
ENV BOLT4SCM_HOME=$HOME/bolt4scm

RUN chmod -R ugo+rw $HOME && chmod +x /start.sh

USER $USER_ID

# This entry point ensures that dumb-init is run
ENTRYPOINT [ "docker-entrypoint.sh" ]
CMD [ "./start.sh" ]
