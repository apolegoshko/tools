FROM alpine:3.23.3

ARG user=user group=default
ARG STERN_VERSION=1.33.1
ARG KUBECTX_VERSION=0.11.0
ARG FZF_VERSION=0.72.0

RUN apk update && apk add bash bash-completion curl wget netcat-openbsd tcpdump doas \
    sed gnupg tcpdump redis postgresql tmux ca-certificates python3 kubectl go && apk cache clean

RUN addgroup $group && adduser --disabled-password --gecos "" --shell /bin/bash --ingroup $group $user
RUN echo "permit nopass $user cmd tcpdump" >> /etc/doas.conf


RUN ARCH=$(uname -m) && \
    case "$ARCH" in \
        x86_64) ARCH=amd64 ;; \
        aarch64) ARCH=arm64 ;; \
    esac && \
    curl -L https://github.com/stern/stern/releases/download/v${STERN_VERSION}/stern_${STERN_VERSION}_linux_${ARCH}.tar.gz \
    | tar -xz -C /usr/local/bin stern && \
    chmod +x /usr/local/bin/stern && \
    curl -L https://github.com/junegunn/fzf/releases/download/v${FZF_VERSION}/fzf-${FZF_VERSION}-linux_${ARCH}.tar.gz \
    | tar -xz -C /usr/local/bin fzf && \
    chmod +x /usr/local/bin/fzf

RUN ARCH=$(uname -m) && \
    case "$ARCH" in \
        x86_64) ARCH=x86_64 ;; \
        aarch64) ARCH=arm64 ;; \
    esac && \
    curl -L https://github.com/ahmetb/kubectx/releases/download/v${KUBECTX_VERSION}/kubectx_v${KUBECTX_VERSION}_linux_${ARCH}.tar.gz \
    | tar -xz -C /usr/local/bin kubectx && \
    curl -L https://github.com/ahmetb/kubectx/releases/download/v${KUBECTX_VERSION}/kubens_v${KUBECTX_VERSION}_linux_${ARCH}.tar.gz \
    | tar -xz -C /usr/local/bin kubens && \
    chmod +x /usr/local/bin/kubectx /usr/local/bin/kubens

USER user
WORKDIR /home/user
COPY --chown=$user:$group bashrc .bashrc
