ARG BUILD_FROM=ghcr.io/home-assistant/amd64-base:3.18

ARG BALENA_ARCH=amd64

# Build process from: https://git.alpinelinux.org/aports/tree/testing/librespot/APKBUILD
FROM alpine:edge as librespot-builder
WORKDIR /

RUN apk add --no-cache --repository=http://dl-cdn.alpinelinux.org/alpine/edge/testing/ \
    librespot=0.4.2-r4


FROM $BUILD_FROM AS run

RUN apk add --no-cache \
    snapcast=0.27.0-r3

COPY --from=librespot-builder /usr/bin/librespot /usr/bin/librespot

ENTRYPOINT [ "/init" ]
CMD []
COPY root /
