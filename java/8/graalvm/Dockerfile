FROM        --platform=$TARGETOS/$TARGETARCH ubuntu:noble

LABEL       author="Garrett Summerfield" maintainer="mail@garrettsummerfi3ld.io"

LABEL       org.opencontainers.image.source="https://github.com/garrettsummerfi3ld/images"
LABEL       org.opencontainers.image.licenses=MIT

ARG         TARGETPLATFORM
ARG         GRAAL_VERSION=21.2.0
ARG         JAVA_VERSION=8

RUN         apt update \
            && apt install -y \
                curl \
                lsof \
                ca-certificates \
                openssl \
                git \
                tar \
                sqlite3 \
                fontconfig \
                tzdata \
                iproute2 \
                libfreetype6 \
                tini \
                zip \
                unzip \
                libstdc++6

RUN         curl --retry 3 -Lfso /tmp/graalvm.tar.gz https://github.com/graalvm/graalvm-ce-builds/releases/download/vm-${GRAAL_VERSION}/graalvm-ce-java${JAVA_VERSION}-linux-amd64-${GRAAL_VERSION}.tar.gz \
            && mkdir -p /opt/java/graalvm \
            && cd /opt/java/graalvm \
            && tar -xf /tmp/graalvm.tar.gz --strip-components=1 \
            && export PATH="/opt/java/graalvm/bin:$PATH" \
            && rm -rf /var/lib/apt/lists/* \
            && rm -rf /tmp/graalvm.tar.gz

ENV        JAVA_HOME=/opt/java/graalvm
ENV        PATH="$PATH:/opt/java/graalvm/bin"

## Setup user and working directory
RUN         useradd -m -d /home/container container
USER        container
ENV         USER=container HOME=/home/container
WORKDIR     /home/container

STOPSIGNAL SIGINT

COPY        --chown=container:container ./../../entrypoint.sh /entrypoint.sh
RUN         chmod +x /entrypoint.sh
ENTRYPOINT    ["/usr/bin/tini", "-g", "--"]
CMD         ["/entrypoint.sh"]
