ARG BUILD_FROM=ghcr.io/hassio-addons/base/amd64:10.1.0
# hadolint ignore=DL3006
FROM ${BUILD_FROM}

# Set shell
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

# Copy Python requirements file
COPY requirements.txt /tmp/

# Setup base
ARG BUILD_ARCH=amd64
# hadolint ignore=DL3003
RUN \
    apk add --no-cache --virtual .build-dependencies \
        build-base=0.5-r2 \
        cmake=3.20.3-r0 \
        py3-wheel=0.36.2-r2 \
        python3-dev=3.9.5-r1 \
        zlib-dev=1.2.11-r3 \
    \
    && apk add --no-cache \
        py3-pip=20.3.4-r1 \
        py3-bcrypt=3.2.0-r3 \
        python3=3.9.5-r1 \
        zlib=1.2.11-r3 \
        jpeg-dev=9d-r1 \
        libpng-dev=1.6.37-r1 \
        ffmpeg-dev=4.4-r1 \
    \
    && pip3 install --no-cache-dir -r /tmp/requirements.txt \
    \
    && find /usr/local \
        \( -type d -a -name test -o -name tests -o -name '__pycache__' \) \
        -o \( -type f -a -name '*.pyc' -o -name '*.pyo' \) \
        -exec rm -rf '{}' + \
    \
    && apk del --no-cache --purge .build-dependencies

# Copy root filesystem
COPY rootfs /

# Build arguments
ARG BUILD_ARCH
ARG BUILD_DATE
ARG BUILD_DESCRIPTION
ARG BUILD_NAME
ARG BUILD_REF
ARG BUILD_REPOSITORY
ARG BUILD_VERSION

# Labels
LABEL \
    io.hass.name="${BUILD_NAME}" \
    io.hass.description="${BUILD_DESCRIPTION}" \
    io.hass.arch="${BUILD_ARCH}" \
    io.hass.type="addon" \
    io.hass.version=${BUILD_VERSION} \
    maintainer="Rami Mousleh" \
    org.opencontainers.image.title="${BUILD_NAME}" \
    org.opencontainers.image.description="${BUILD_DESCRIPTION}" \
    org.opencontainers.image.vendor="Home Assistant Add-ons" \
    org.opencontainers.image.authors="Rami Mousleh" \
    org.opencontainers.image.licenses="MIT" \
    org.opencontainers.image.url="https://addons.community" \
    org.opencontainers.image.source="https://github.com/${BUILD_REPOSITORY}" \
    org.opencontainers.image.documentation="https://github.com/${BUILD_REPOSITORY}/blob/main/README.md" \
    org.opencontainers.image.created=${BUILD_DATE} \
    org.opencontainers.image.revision=${BUILD_REF} \
    org.opencontainers.image.version=${BUILD_VERSION}
