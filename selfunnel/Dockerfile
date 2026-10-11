ARG BUILD_FROM
FROM $BUILD_FROM

RUN apk add --no-cache curl bash jq ca-certificates

# Единый источник версии frp: манифест /opt/selfunnel/data/frp_manifest.json (scripts/frp_sync.py).
# SHA256 сверены по манифесту 2026-10-11 (0.71.0 linux amd64/arm64).
ARG FRP_VERSION="0.71.0"
ARG FRP_SHA256_AMD64="f79fff8de3089ec711ff8bdd4b73e00dfe491a1c3d754983c8b0f8d58c21b068"
ARG FRP_SHA256_AARCH64="6e8e45fd0c7514b636fd8d049212f8a5715e8b33c412cf968988252e7a8a00f2"
# TARGETARCH ставится HA addon-builder (buildkit); fallback — uname -m
ARG TARGETARCH

# Определяем архитектуру frp: TARGETARCH -> amd64/arm64, иначе uname -m
RUN ARCH=${TARGETARCH:-$(uname -m)} \
    && case "$ARCH" in amd64|x86_64) FRP_ARCH=amd64; SHA="$FRP_SHA256_AMD64";; \
       arm64|aarch64) FRP_ARCH="arm64"; SHA="$FRP_SHA256_AARCH64";; \
       *) echo "Unsupported architecture: $ARCH"; exit 1;; esac \
    && curl -fsSL -A "selfunnel-addon/1.1.0" "https://github.com/fatedier/frp/releases/download/v${FRP_VERSION}/frp_${FRP_VERSION}_linux_${FRP_ARCH}.tar.gz" \
        -o /tmp/frp.tar.gz \
    && echo "${SHA}  /tmp/frp.tar.gz" | sha256sum -c - \
    && tar -xzf /tmp/frp.tar.gz -C /tmp \
    && cp "/tmp/frp_${FRP_VERSION}_linux_${FRP_ARCH}/frpc" /usr/local/bin/frpc \
    && chmod +x /usr/local/bin/frpc \
    && rm -rf /tmp/frp*

COPY run.sh /
RUN chmod a+x /run.sh

CMD ["/run.sh"]
