# Build frontend first
FROM node:11 AS frontend
ARG TARGETPLATFORM
ARG BUILDPLATFORM
ARG VERSION
ARG CHANNEL

RUN \
    curl -fsSL "https://api.github.com/repos/alice-lg/alice-lg/tarball/refs/tags/${VERSION}" | tar xzf - -C /tmp \
    && mkdir /src \
    && mv /tmp/alice-lg* /src/alice-lg \
    && chown -R root:root /src/alice-lg \
    && chmod -R 755 /src/alice-lg \
    && rm -rf /tmp/*

# Install dependencies
WORKDIR /src/alice-lg/client


RUN npm install -g gulp@4.0.0
RUN npm install -g gulp-cli
RUN yarn install

# Build frontend
RUN DISABLE_LOGGING=1 NODE_ENV=production /usr/local/bin/gulp

FROM golang:1.19 AS backend

RUN mkdir -p /src/alice-lg
WORKDIR /src/alice-lg
COPY --from=frontend /src/alice-lg .

RUN go mod download

WORKDIR /src/alice-lg/cmd/alice-lg
RUN make alpine

FROM ghcr.io/nicko170/alpine:3.16.3

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

COPY --from=backend /src/alice-lg/cmd/alice-lg/alice-lg-linux-amd64 /app/alice-lg

USER user
COPY ./apps/alice-lg/alice.conf /config/alice.conf
COPY ./apps/alice-lg/entrypoint.sh /entrypoint.sh
CMD ["/entrypoint.sh"]

LABEL org.opencontainers.image.source="https://github.com/alice-lg/alice-lg"