FROM python:3.7-slim-stretch

ARG CONFTEST_VERSION=0.21.0
ARG KONSTRAINT_VERSION=0.9.2
ARG KUSTOMIZE_VERSION=3.8.7
ARG KUBECTL_VERSION=1.18.4
ARG OPA_VERSION=0.24.0

# Install deps all in one step
RUN apt-get update -y && \
  apt-get install -y \
  apt-transport-https \
  build-essential \
  ca-certificates \
  curl \
  git \
  gnupg \
  software-properties-common \
  wget \
  zsh
# Get all of the signatures we need all at once.
RUN curl -fsSL https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add - && \
  # Add conftest
  wget https://github.com/open-policy-agent/conftest/releases/download/v${CONFTEST_VERSION}/conftest_${CONFTEST_VERSION}_Linux_x86_64.tar.gz && \
  tar xzf conftest_${CONFTEST_VERSION}_Linux_x86_64.tar.gz && \
  mv conftest /usr/bin/ && \
  chmod +x /usr/bin/conftest && \
  # Add konstraint
  wget https://github.com/plexsystems/konstraint/releases/download/v${KONSTRAINT_VERSION}/konstraint-linux-amd64 && \
  mv konstraint-linux-amd64 /usr/bin/konstraint && \
  chmod +x /usr/bin/konstraint && \
  # Add kustomize
  wget https://github.com/kubernetes-sigs/kustomize/releases/download/kustomize%2Fv${KUSTOMIZE_VERSION}/kustomize_v${KUSTOMIZE_VERSION}_linux_amd64.tar.gz && \
  tar xzf  kustomize_v${KUSTOMIZE_VERSION}_linux_amd64.tar.gz && \
  mv kustomize /usr/bin/ && \
  chmod +x /usr/bin/kustomize && \
  # Add kubectl
  curl -sfL -o /usr/bin/kubectl https://storage.googleapis.com/kubernetes-release/release/v${KUBECTL_VERSION}/bin/linux/amd64/kubectl && \
  chmod +x /usr/bin/kubectl && \
  # Add OPA
  curl -L -o /usr/bin/opa https://openpolicyagent.org/downloads/v${OPA_VERSION}/opa_linux_amd64 && \
  chmod +x /usr/bin/opa

# Install Helm
# Explicitly set env variables that helm reads to their defaults, so that subsequent calls to
# helm will find the stable repo even if $HOME points to something other than /root
# (e.g. in GitHub actions where $HOME points to /github/home).
ENV XDG_CONFIG_HOME=/root/.config
ENV XDG_CACHE_HOME=/root/.cache
RUN curl -L https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3 | bash && \
  helm repo add stable https://charts.helm.sh/stable && \
  helm repo update
