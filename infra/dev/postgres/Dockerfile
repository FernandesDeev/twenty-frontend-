ARG PG_MAIN_VERSION=14

FROM postgres:${PG_MAIN_VERSION} as postgres

ARG PG_MAIN_VERSION
ARG PG_GRAPHQL_VERSION=1.3.0
ARG TARGETARCH

RUN set -eux; \
    ARCH="$(dpkg --print-architecture)"; \
    case "${ARCH}" in \
       aarch64|arm64) \
         TARGETARCH='arm64'; \
         ;; \
       amd64|x86_64) \
         TARGETARCH='amd64'; \
         ;; \
       *) \
         echo "Unsupported arch: ${ARCH}"; \
         exit 1; \
         ;; \
    esac;

RUN apt update && apt install -y curl

# Install precompiled pg_graphql extensions
RUN curl -L "https://github.com/supabase/pg_graphql/releases/download/v${PG_GRAPHQL_VERSION}/pg_graphql-v${PG_GRAPHQL_VERSION}-pg${PG_MAIN_VERSION}-${TARGETARCH}-linux-gnu.deb" -o pg_graphql.deb
RUN dpkg --install pg_graphql.deb

COPY init.sql /docker-entrypoint-initdb.d/
