# FROM redhat/ubi9-minimal:9.2-750
FROM registry.access.redhat.com/ubi8-minimal:8.8-1072

ARG TARGETARCH

ENV HOME="/" \
    OS_ARCH="${TARGETARCH:-amd64}" \
    OS_FLAVOUR="ubi8" \
    OS_NAME="linux"

COPY prebuildfs /
COPY redis-7.2.1-0-linux-amd64-el8.tar.gz /
SHELL ["/bin/bash", "-o", "pipefail", "-c"]
# Install required system packages and dependencies
RUN microdnf install procps-ng ca-certificates tar gzip hostname findutils -y
RUN mkdir -p /opt/bitnami && mkdir -p /tmp/bitnami/pkg/cache/ && cd /tmp/bitnami/pkg/cache/ && \
    mv /redis-7.2.1-0-linux-amd64-el8.tar.gz /tmp/bitnami/pkg/cache/redis-7.2.1-0-linux-amd64-el8.tar.gz && \
    tar -zxf redis-7.2.1-0-linux-amd64-el8.tar.gz -C /opt/bitnami --strip-components=2 --no-same-owner --wildcards '*/files' && \
    rm -rf redis-7.2.1-0-linux-amd64-el8.tar.gz ;

RUN microdnf remove tar gzip -y && \
    microdnf update && microdnf upgrade -y && \
    microdnf clean all

RUN chmod g+rwX /opt/bitnami

COPY rootfs /
RUN /opt/bitnami/scripts/redis-cluster/postunpack.sh
ENV APP_VERSION="7.2.1" \
    BITNAMI_APP_NAME="redis-cluster" \
    PATH="/opt/bitnami/redis/bin:$PATH"

EXPOSE 6379

USER 1001
ENTRYPOINT [ "/opt/bitnami/scripts/redis-cluster/entrypoint.sh" ]
CMD [ "/opt/bitnami/scripts/redis-cluster/run.sh" ]


# docker run --rm -it --platform linux/amd64 redis-cluster