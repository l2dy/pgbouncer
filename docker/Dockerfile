# bitnami/pgbouncer:1.20.1
FROM --platform=$BUILDPLATFORM bitnami/pgbouncer@sha256:710da11a466f98b90380fb8e02b487aacf8ef33b07acabb402067c122ac63e8d AS source

USER root

WORKDIR /buildroot

RUN apt-get update && apt-get install -y \
  git \
  autoconf automake libtool pkg-config \
  && rm -rf /var/lib/apt/lists/*

RUN git clone --recurse-submodules -b bitnami-container https://github.com/l2dy/pgbouncer.git \
  && cd pgbouncer && ./autogen.sh

FROM bitnami/pgbouncer@sha256:710da11a466f98b90380fb8e02b487aacf8ef33b07acabb402067c122ac63e8d AS builder

USER root

RUN apt-get update && apt-get install -y \
  make gcc pkg-config libevent-dev libssl-dev python3-minimal pandoc \
  && rm -rf /var/lib/apt/lists/*

COPY --from=source /buildroot/pgbouncer /buildroot/pgbouncer/
WORKDIR /buildroot/pgbouncer

RUN ./configure --prefix=/opt/bitnami/pgbouncer && make && make install

FROM bitnami/pgbouncer@sha256:710da11a466f98b90380fb8e02b487aacf8ef33b07acabb402067c122ac63e8d

COPY --from=builder /opt/bitnami/pgbouncer/bin/pgbouncer /opt/bitnami/pgbouncer/bin/pgbouncer
