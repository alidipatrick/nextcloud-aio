# syntax=docker/dockerfile:latest
FROM python:3.12.3-alpine3.19

COPY --chmod=775 start.sh /start.sh

ENV RECORDING_VERSION v0.1
ENV ALLOW_ALL false
ENV HPB_PROTOCOL https
ENV SKIP_VERIFY false
ENV HPB_PATH /standalone-signaling/

RUN set -ex; \
    apk add --no-cache \
        ca-certificates \
        tzdata \
        bash \
        xvfb \
        ffmpeg \
        firefox \
        bind-tools \
        netcat-openbsd \
        git \
        wget \
        shadow \
        pulseaudio \
        openssl \
        build-base \
        linux-headers; \
    # chromium chromium-chromedriver?
    apk add --no-cache geckodriver --repository http://dl-cdn.alpinelinux.org/alpine/edge/community; \
    useradd -d /tmp --system recording; \
# Give root a random password
    echo "root:$(openssl rand -base64 12)" | chpasswd; \
    git clone --recursive https://github.com/nextcloud/nextcloud-talk-recording --depth=1 --single-branch --branch "$RECORDING_VERSION" /src; \
    python3 -m pip install --no-cache-dir /src; \
    rm -rf /src; \
    touch /etc/recording.conf; \
    chown recording:recording -R \
        /tmp /etc/recording.conf; \
    mkdir -p /conf; \
    chmod 777 /conf; \
    chmod 777 /tmp; \
    apk del --no-cache \
        git \
        wget \
        shadow \
        openssl \
        build-base \
        linux-headers;

WORKDIR /tmp
USER recording
ENTRYPOINT ["/start.sh"]
CMD ["python", "-m", "nextcloud.talk.recording", "--config", "/conf/recording.conf"]

HEALTHCHECK CMD nc -z localhost 1234 || exit 1
LABEL com.centurylinklabs.watchtower.enable="false"
