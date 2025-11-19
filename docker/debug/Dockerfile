FROM debian:stable-slim

ENV user=user group=default
ENV KUBECTL_VERSION=1.34

RUN apt-get update
RUN apt-get install -y curl wget zsh netcat-openbsd sed \
    neovim postgresql-client python3 apt-transport-https ca-certificates gnupg
RUN curl -fsSL "https://pkgs.k8s.io/core:/stable:/v$KUBECTL_VERSION/deb/Release.key" | gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg
RUN chmod 644 /etc/apt/keyrings/kubernetes-apt-keyring.gpg
RUN echo "deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v$KUBECTL_VERSION/deb/ /" | tee /etc/apt/sources.list.d/kubernetes.list
RUN chmod 644 /etc/apt/sources.list.d/kubernetes.list

RUN curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | gpg --dearmor -o /usr/share/keyrings/cloud.google.gpg
RUN echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | tee -a /etc/apt/sources.list.d/google-cloud-sdk.list
RUN apt-get update && apt-get install google-cloud-cli kubectl google-cloud-cli-gke-gcloud-auth-plugin kubectx -y

RUN curl -s https://fluxcd.io/install.sh | bash

RUN apt-get clean
RUN addgroup $group && adduser --disabled-password --gecos "" --shell /bin/zsh --ingroup $group $user
USER user
WORKDIR /home/user
ADD ./config/zshrc .zshrc
