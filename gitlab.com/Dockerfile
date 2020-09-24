FROM ubuntu:bionic-20200903

SHELL ["/bin/bash", "-euo", "pipefail", "-c"]

ARG DOCKER_VERSION=19.03.13
ARG AWS_CLI_VERSION=2.0.51
ARG K8S_VERSION=1.17.9

# hadolint ignore=DL3008
RUN export DEBIAN_FRONTEND=noninteractive \
 && apt-get update \
 && apt-get install --no-install-recommends -y \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg \
    unzip \
 && apt-get autoremove -y \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

# Install latest Docker

RUN curl -fsSL "https://download.docker.com/linux/ubuntu/gpg" | apt-key add - \
 && echo 'deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable' > /etc/apt/sources.list.d/docker.list \
 && apt-get -y update \
 && apt-get install --no-install-recommends -y docker-ce=5:${DOCKER_VERSION}~* \
 && apt-get clean \
 && rm -rf /var/lib/apt/lists/*

WORKDIR /tmp

# Install latest AWS CLI
RUN curl -fsSLO https://awscli.amazonaws.com/awscli-exe-linux-x86_64-${AWS_CLI_VERSION}.zip \
 && unzip awscli-exe-linux-x86_64-${AWS_CLI_VERSION}.zip \
 && ./aws/install \
 && rm -rf /tmp/*

WORKDIR /usr/local/bin

# Install kubectl
RUN curl -fsSLO https://storage.googleapis.com/kubernetes-release/release/v${K8S_VERSION}/bin/linux/amd64/kubectl \
 && chmod +x kubectl \
 && kubectl version --client=true

# Install AWS IAM authenticator
RUN curl -fsSLO https://amazon-eks.s3.us-west-2.amazonaws.com/${K8S_VERSION}/2020-08-04/bin/linux/amd64/aws-iam-authenticator \
 && chmod +x aws-iam-authenticator \
 && aws-iam-authenticator version

WORKDIR /root
