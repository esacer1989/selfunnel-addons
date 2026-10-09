ARG BUILD_FROM
FROM $BUILD_FROM

RUN apk add --no-cache curl bash jq ca-certificates

ARG FRP_VERSION="0.58.0"
ARG FRP_SHA256_AMD64=""
ARG FRP_SHA256_AARCH64=""
# TARGETARCH ставится HA addon-builder (buildkit); fallback — uname -m
ARG TARGETARCH

# Определяем архитектуру frp: TARGETARCH -> amd64/arm64, иначе uname -m
RUN ARCH=${TARGETARCH:-$(uname -m)} \
    && case "$ARCH" in amd64|x86_64) FRP_ARCH=amd64; SHA="$FRP_SHA256_AMD64";; \
       arm64|aarch64) FRP_ARCH="arm64"; SHA="$FRP_SHA256_AARCH64";; \
       *) echo "Unsupported architecture: $ARCH"; exit 1;; esac \
    && curl -fsSL "https://github.com/fatedier/frp/releases/download/v${FRP_VERSION}/frp_${FRP_VERSION}_linux_${FRP_ARCH}.tar.gz" \
        -o /tmp/frp.tar.gz \
    && if [ -n "$SHA" ]; then echo "${SHA}  /tmp/frp.tar.gz" | sha256sum -c -; else echo "WARNING: FRP_SHA256_${FRP_ARCH} not set — skipping checksum verification"; fi \
    && tar -xzf /tmp/frp.tar.gz -C /tmp \
    && cp "/tmp/frp_${FRP_VERSION}_linux_${FRP_ARCH}/frpc" /usr/local/bin/frpc \
    && chmod +x /usr/local/bin/frpc \
    && rm -rf /tmp/frp*

COPY run.sh /
RUN chmod a+x /run.sh

CMD ["/run.sh"]
