ARG BUILD_FROM=ghcr.io/home-assistant/amd64-base:3.18

ARG BALENA_ARCH=amd64

# Build process from: https://git.alpinelinux.org/aports/tree/testing/librespot/APKBUILD
FROM alpine:edge as librespot-builder
WORKDIR /

RUN apk add --no-cache --repository=http://dl-cdn.alpinelinux.org/alpine/edge/testing/ \
    librespot=0.4.2-r3


FROM $BUILD_FROM AS run

RUN apk add --no-cache \
    snapcast=0.27.0-r3

#RUN cargo install \
#    --locked \
#    --no-default-features \
#    --features with-dns-sd \
#    --root /usr \
#    --bin librespot \
#    --version 0.4.2 \
#    --verbose \
#    -- librespot
#COPY --from=mopidy-iris /root/.local/lib/python3.11/site-packages /usr/lib/python3.11/site-packages/
#COPY --from=mopidy-mpd /root/.local/lib/python3.11/site-packages /usr/lib/python3.11/site-packages/
#COPY --from=mopidy-local /root/.local/lib/python3.11/site-packages /usr/lib/python3.11/site-packages/
COPY --from=librespot-builder /usr/bin/librespot /usr/bin/librespot

ENTRYPOINT [ "/init" ]
CMD []
COPY root /
