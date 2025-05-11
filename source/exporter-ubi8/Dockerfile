FROM registry.red-soft.ru/ubi8/ubi-minimal

ARG TARGETARCH

ENV HOME="/" \
    OS_ARCH="${TARGETARCH:-amd64}" \
    OS_FLAVOUR="ubi-minimal" \
    OS_NAME="linux"

COPY prebuildfs /
COPY redis-exporter-1.54.0-0-linux-amd64-el8.tar.gz /
SHELL ["/bin/bash", "-o", "pipefail", "-c"]
# Install required system packages and dependencies
RUN microdnf install procps-ng ca-certificates tar gzip hostname findutils openssl1.1 -y && \ 
    mkdir -p /opt/bitnami && mkdir -p /tmp/bitnami/pkg/cache/ && cd /tmp/bitnami/pkg/cache/ && \
    mv /redis-exporter-1.54.0-0-linux-amd64-el8.tar.gz /tmp/bitnami/pkg/cache/redis-exporter-1.54.0-0-linux-amd64-el8.tar.gz && \
    tar -zxf redis-exporter-1.54.0-0-linux-amd64-el8.tar.gz -C /opt/bitnami --strip-components=2 --no-same-owner --wildcards '*/files' && \
    rm -rf redis-exporter-1.54.0-0-linux-amd64-el8.tar.gz && \
    microdnf remove tar gzip -y && \
    microdnf clean all

# RUN microdnf remove tar gzip -y && \
#     microdnf update && microdnf upgrade -y && \
#     microdnf clean all

RUN chmod g+rwX /opt/bitnami


ENV APP_VERSION="1.54.0" \
    BITNAMI_APP_NAME="redis-exporter" \
    PATH="/opt/bitnami/redis-exporter/bin:$PATH"

EXPOSE 9121

WORKDIR /opt/bitnami/redis-exporter
USER 1001
ENTRYPOINT [ "redis_exporter" ]

