ARG BUILD_TAG=master

FROM docker.io/library/golang:alpine as builder
ARG BUILD_TAG
WORKDIR /go/src/

RUN apk add --no-cache -U git \
    && git clone https://github.com/acavella/gorevoke \
    && cd gorevoke && git checkout ${BUILD_TAG} \
    && /bin/sh /go/src/gorevoke/build/docker/bin/install.sh ${BUILD_TAG}

FROM registry.access.redhat.com/ubi9-minimal:9.5 as runner

LABEL org.opencontainers.image.title="GoRevoke"
LABEL org.opencontainers.image.version=${BUILD_TAG}
LABEL org.opencontainers.image.author="Tony Cavella <tony@cavella.com>"
LABEL org.opencontainers.image.url="https://github.com/acavella/gorevoke"
LABEL org.opencontainers.image.description="GoRevoke is a standalone Certificate Revocation List Distrution Point written in Go, designed to be lightweight and fully self-contained."
LABEL org.opencontainers.image.licenses="MIT"

COPY --from=builder /usr/local/bin/gorevoke/gorevoke /usr/local/bin/gorevoke

EXPOSE 4000
CMD [ "/usr/local/bin/gorevoke" ]
