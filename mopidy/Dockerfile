ARG BUILD_FROM=ghcr.io/home-assistant/amd64-base:3.18

FROM $BUILD_FROM AS build-base

RUN apk add --no-cache \
    python3=3.11.5-r0 \
    py3-gst=1.22.5-r0 \
    py3-pip=23.1.2-r0 \
    mopidy=3.4.1-r1 \
    && ln -s /usr/lib/libpython3.11.so.1.0 /usr/lib/libpython3.11.so


FROM build-base AS mopidy-iris

RUN pip3 install --no-cache-dir --user Mopidy-Iris==3.68.0 &&\
    ls -al /root/.local/lib/python3.11/site-packages


FROM build-base AS mopidy-mpd

RUN pip3 install --no-cache-dir --user Mopidy-MPD==3.3.0 &&\
    ls -al /root/.local/lib/python3.11/site-packages

FROM build-base AS mopidy-local

RUN pip3 install --no-cache-dir --user Mopidy-Local==3.2.1 &&\
    ls -al /root/.local/lib/python3.11/site-packages


FROM $BUILD_FROM AS run

RUN apk add --no-cache \
    python3=3.11.5-r0 \
    py3-gst=1.22.5-r0 \
    mopidy=3.4.1-r1 \
    ca-certificates=20230506-r0 \
    sudo=1.9.13_p3-r2 \
    && ln -s /usr/lib/libpython3.11.so.1.0 /usr/lib/libpython3.11.so

COPY --from=mopidy-iris /root/.local/lib/python3.11/site-packages /usr/lib/python3.11/site-packages/
COPY --from=mopidy-mpd /root/.local/lib/python3.11/site-packages /usr/lib/python3.11/site-packages/
COPY --from=mopidy-local /root/.local/lib/python3.11/site-packages /usr/lib/python3.11/site-packages/

RUN echo "root ALL=NOPASSWD: /usr/lib/python3.11/site-packages/mopidy_iris/system.sh" >> /etc/sudoers

ENTRYPOINT [ "/init" ]
CMD []
COPY root /
