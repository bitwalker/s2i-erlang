FROM bitwalker/s2i-alpine-base:1.0

MAINTAINER Paul Schoenfelder <paulschoenfelder@gmail.com>

EXPOSE 5000
EXPOSE 8080

ENV \
  PORT=5000 \
  NPM_VERSION=3 \
  NODE_ENV=production \
  REFRESHED_AT=2016-04-9

LABEL \
  io.k8s.description="Platform for building and running Erlang applications" \
  io.k8s.display-name="Erlang v18.3, Node.js v4.4.2" \
  io.openshift.expose-services="5000:http,8080:tcp" \
  io.openshift.tags="builder,erlang,erlang-18.3" \
  com.redhat.deployments-dir="$HOME"

RUN apk add --no-cache --update \
  ncurses-libs erlang erlang-dev erlang-kernel erlang-hipe erlang-compiler \
  erlang-stdlib erlang-erts erlang-tools erlang-syntax-tools erlang-sasl \
  erlang-crypto erlang-public-key erlang-ssl erlang-ssh erlang-asn1 erlang-inets \
  erlang-mnesia erlang-odbc erlang-xmerl erlang-erl-interface erlang-parsetools erlang-eunit \
  nodejs=4.4.2-r0 && \
  wget https://s3.amazonaws.com/rebar3/rebar3 && chmod +x rebar3 && mv rebar3 /usr/bin && \
  npm install npm -g --no-progress && \
  rm -rf /var/cache/apk/*

ENV PATH=${HOME}/node_modules/.bin:$PATH

# Copy the S2I scripts
COPY ./bin/ $STI_SCRIPTS_PATH

# Drop the root user and make the content of $HOME owned by user 1001
RUN chown -R 1001:0 /opt/app-root $STI_SCRIPTS_PATH
USER 1001

# Set the default CMD to print the usage for this iamge
CMD $STI_SCRIPTS_PATH/usage
