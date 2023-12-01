ARG BUILD_FROM=ghcr.io/home-assistant/amd64-base:3.18

FROM $BUILD_FROM AS build-base

RUN apk add --no-cache \
    python3=3.11.6-r0 \
    py3-gst=1.22.5-r0 \
    py3-pip=23.1.2-r0 \
    mopidy=3.4.1-r1 \
    && ln -s /usr/lib/libpython3.11.so.1.0 /usr/lib/libpython3.11.so


FROM build-base AS mopidy-mpd

RUN pip3 install --no-cache-dir --user Mopidy-MPD==3.3.0 &&\
    ls -al /root/.local/lib/python3.11/site-packages

FROM $BUILD_FROM AS run

RUN apk add --no-cache \
    python3=3.11.6-r0 \
    py3-gst=1.22.5-r0 \
    mopidy=3.4.1-r1 \
    ca-certificates=20230506-r0 \
    sudo=1.9.13_p3-r2 \
    && ln -s /usr/lib/libpython3.11.so.1.0 /usr/lib/libpython3.11.so

COPY --from=mopidy-mpd /root/.local/lib/python3.11/site-packages /usr/lib/python3.11/site-packages/

ENTRYPOINT [ "/init" ]
CMD []
COPY root /
