FROM gcr.io/kaniko-project/executor:debug AS kaniko
FROM grababyte/alpine-base:0.11.0

LABEL devops.grababyte.image.authors="GrabAByte"

ENV ARCH amd64
ENV HELM_VERSION 3.15.0
ENV ISTIO_VERSION 1.22.0
ENV K8S_VERSION 1.30.1
ENV USER alpine
ENV PATH "${PATH}:/home/${USER}/istio-${ISTIO_VERSION}/bin:/home/${USER}/.local/bin:/kaniko"

## kaniko
COPY --from=kaniko /kaniko/ /kaniko/
ENV DOCKER_CONFIG /kaniko/.docker/
ENV DOCKER_CREDENTIAL_GCR_CONFIG /kaniko/.config/gcloud/docker_credential_gcr_config.json
ENV SSL_CERT_DIR /kaniko/ssl/certs

USER root

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

RUN \
    curl -O https://get.helm.sh/helm-v${HELM_VERSION}-linux-${ARCH}.tar.gz && \
      tar xvzf helm-v${HELM_VERSION}-linux-${ARCH}.tar.gz && \
      mv linux-${ARCH}/* /usr/local/bin/ && \
      rmdir linux-${ARCH} && \
      chmod 755 /usr/local/bin/helm && \
    curl -O https://istio.io/downloadIstio | ISTIO_VERSION=${ISTIO_VERSION} TARGET_ARCH=${ARCH} sh - && \
    curl -O https://dl.k8s.io/release/v${K8S_VERSION}/bin/linux/amd64/kubectl && \
      install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl && \
    apk add --no-cache \
      aws-cli==2.13.25-r0 \
      buildkit==0.12.5-r3 \
      containerd-ctr==1.7.10-r3 && \
    rm -f helm-v${HELM_VERSION}-linux-${ARCH}.tar.gz

USER "${USER}"
