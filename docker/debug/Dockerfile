FROM alpine:3.22

ENV user=user group=default

RUN apk update
RUN apk add curl wget zsh netcat-openbsd \
    neovim bind-tools postgresql-client valkey-cli
RUN apk cache clean
RUN addgroup -S $group && adduser -D -s /bin/zsh $user -G $group
USER user
ADD ./config/zshrc /home/user/.zshrc
