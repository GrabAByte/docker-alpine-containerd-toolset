FROM grababyte/alpine-base:0.11.0

LABEL devops.grababyte.image.authors="GrabAByte"

ENV ARCH amd64
ENV HELM_VERSION 3.15.0
ENV ISTIO_VERSION 1.22.0
ENV K8S_VERSION 1.30.1
ENV PACKER_VERSION 1.10.3
ENV TERRAFORM_VERSION 1.4.6
ENV USER alpine
ENV PATH "${PATH}:/home/${USER}/istio-${ISTIO_VERSION}/bin:/home/${USER}/.local/bin"

USER root

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

RUN \
    curl -LO https://get.helm.sh/helm-v${HELM_VERSION}-linux-${ARCH}.tar.gz && \
      tar xvzf helm-v${HELM_VERSION}-linux-${ARCH}.tar.gz && \
      mv linux-${ARCH}/* /usr/local/bin/ && \
      rmdir linux-${ARCH} && \
      chmod 755 /usr/local/bin/helm && \
    curl -L https://istio.io/downloadIstio | ISTIO_VERSION=${ISTIO_VERSION} TARGET_ARCH=${ARCH} sh - && \
    curl -LO https://dl.k8s.io/release/v${K8S_VERSION}/bin/linux/amd64/kubectl && \
      install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl && \
    curl -LO https://releases.hashicorp.com/packer/${PACKER_VERSION}/packer_${PACKER_VERSION}_linux_${ARCH}.zip && \
      unzip packer_${PACKER_VERSION}_linux_${ARCH}.zip && \
      mv packer /usr/local/bin && \
      chmod 755 /usr/local/bin/packer && \
    curl -LO https://releases.hashicorp.com/terraform/${TERRAFORM_VERSION}/terraform_${TERRAFORM_VERSION}_linux_${ARCH}.zip && \
      unzip terraform_${TERRAFORM_VERSION}_linux_${ARCH}.zip && \
      mv terraform /usr/local/bin/terraform && \
      chmod 755 /usr/local/bin/terraform && \
    apk add --no-cache \
      ansible-core==2.16.1-r0 \
      aws-cli==2.13.25-r0 && \
    rm -f \
      helm-v${HELM_VERSION}-linux-${ARCH}.tar.gz \
      packer_${PACKER_VERSION}_linux_${ARCH}.zip \
      terraform_${TERRAFORM_VERSION}_linux_${ARCH}.zip

USER "${USER}"
