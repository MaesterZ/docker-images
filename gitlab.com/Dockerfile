FROM ubuntu:20.04

SHELL ["/bin/bash", "-euo", "pipefail", "-c"]

ARG DOCKER_VERSION=20.10.6
ARG AWS_CLI_VERSION=2.2.4
ARG K8S_VERSION=1.20.7
ARG AUTHENTICATOR_VERSION=0.5.3

# hadolint ignore=DL3008
RUN export DEBIAN_FRONTEND=noninteractive \
 && apt-get update \
 && apt-get install --no-install-recommends -y \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    jq \
    unzip \
 && apt-get autoremove -y \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

# Install Docker

RUN curl -fsSL "https://download.docker.com/linux/ubuntu/gpg" | apt-key add - \
 && echo 'deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable' > /etc/apt/sources.list.d/docker.list \
 && apt-get -y update \
 && apt-get install --no-install-recommends -y docker-ce=5:${DOCKER_VERSION}~* \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

WORKDIR /tmp

# Install AWS CLI
RUN curl -fsSLO https://awscli.amazonaws.com/awscli-exe-linux-x86_64-${AWS_CLI_VERSION}.zip \
 && unzip awscli-exe-linux-x86_64-${AWS_CLI_VERSION}.zip \
 && ./aws/install \
 && aws --version \
 && rm -rf /tmp/*

WORKDIR /usr/local/bin

# Install Kubernetes CLI
RUN curl -fsSLO https://storage.googleapis.com/kubernetes-release/release/v${K8S_VERSION}/bin/linux/amd64/kubectl \
 && chmod +x kubectl \
 && kubectl version --client=true

# Install AWS IAM authenticator
RUN curl -fsSL https://github.com/kubernetes-sigs/aws-iam-authenticator/releases/download/v${AUTHENTICATOR_VERSION}/aws-iam-authenticator_${AUTHENTICATOR_VERSION}_linux_amd64 \
         -o aws-iam-authenticator \
 && chmod +x aws-iam-authenticator \
 && aws-iam-authenticator version

WORKDIR /root
