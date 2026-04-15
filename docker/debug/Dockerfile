FROM alpine:3.23.3

ARG user=user group=default

RUN apk update && apk add bash bash-completion curl wget netcat-openbsd \
    sed gnupg tcpdump redis postgresql tmux ca-certificates && apk cache clean

RUN addgroup $group && adduser --disabled-password --gecos "" --shell /bin/bash --ingroup $group $user
USER user
WORKDIR /home/user
COPY --chown=$user:$group bashrc .bashrc
