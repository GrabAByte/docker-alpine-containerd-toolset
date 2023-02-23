FROM grababyte/alpine-base:0.9.1

LABEL devops.grababyte.image.authors="GrabAByte"

ENV ANSIBLE_VERSION 2.14.2
ENV ARCH amd64
ENV CONTAINERD_VERSION 1.6.18
ENV HELM_VERSION 3.11.1
ENV ISTIO_VERSION 1.17.0
ENV K8S_VERSION 1.26.0
ENV PACKER_VERSION 1.8.6
ENV TERRAFORM_VERSION 1.3.9
ENV USER alpine
ENV PATH "${PATH}:/root/istio-${ISTIO_VERSION}/bin"

USER root

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

## TODO: containerd needs some work to fix / may need to install from source
RUN \
    # curl -LO https://github.com/containerd/containerd/releases/download/v${CONTAINERD_VERSION}/containerd-${CONTAINERD_VERSION}-linux-${ARCH}.tar.gz && \
      # tar xzvf containerd-${CONTAINERD_VERSION}-linux-${ARCH}.tar.gz && \
      # mv bin/* /usr/local/bin/ && \
      # rm -rf bin && \
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
    apk add --no-cache python3-dev=3.10.10-r0 && \
    pip3 install --no-cache-dir -U \
      ansible-core==${ANSIBLE_VERSION} \
      awscliv2==2.2.0 \
      azure-cli==2.45.0 \
      yamllint==1.29.0 && \
    rm -f \
      # containerd-${CONTAINERD_VERSION}-linux-${ARCH}.tar.gz \
      helm-v${HELM_VERSION}-linux-${ARCH}.tar.gz \
      packer_${PACKER_VERSION}_linux_${ARCH}.zip \
      terraform_${TERRAFORM_VERSION}_linux_${ARCH}.zip

USER "${USER}"
