FROM marketplace.gcr.io/google/python

RUN apt-get update && apt-get install -y --no-install-recommends \
    bash \
    ca-certificates \
    curl \
    gettext \
    git \
    jq \
    procps \
    wget \
    && rm -rf /var/lib/apt/lists/*

RUN pip3 install \
      wheel \
      pyOpenSSL \
      pyyaml \
      six

RUN mkdir -p /opt/kubectl/1.13 \
    && wget -q -O /opt/kubectl/1.13/kubectl \
        https://storage.googleapis.com/kubernetes-release/release/v1.13.12/bin/linux/amd64/kubectl \
    && chmod 755 /opt/kubectl/1.13/kubectl
RUN mkdir -p /opt/kubectl/1.14 \
    && wget -q -O /opt/kubectl/1.14/kubectl \
        https://storage.googleapis.com/kubernetes-release/release/v1.14.8/bin/linux/amd64/kubectl \
    && chmod 755 /opt/kubectl/1.14/kubectl
RUN mkdir -p /opt/kubectl/1.15 \
    && wget -q -O /opt/kubectl/1.15/kubectl \
        https://storage.googleapis.com/kubernetes-release/release/v1.15.5/bin/linux/amd64/kubectl \
    && chmod 755 /opt/kubectl/1.15/kubectl
RUN mkdir -p /opt/kubectl/1.16 \
    && wget -q -O /opt/kubectl/1.16/kubectl \
        https://storage.googleapis.com/kubernetes-release/release/v1.16.2/bin/linux/amd64/kubectl \
    && chmod 755 /opt/kubectl/1.16/kubectl
RUN ln -s /opt/kubectl/1.14 /opt/kubectl/default

RUN mkdir -p /bin/helm-downloaded \
    && wget -q -O /bin/helm-downloaded/helm.tar.gz \
        https://storage.googleapis.com/kubernetes-helm/helm-v2.10.0-linux-amd64.tar.gz \
    && tar -zxvf /bin/helm-downloaded/helm.tar.gz -C /bin/helm-downloaded \
    && mv /bin/helm-downloaded/linux-amd64/helm /bin/ \
    && rm -rf /bin/helm-downloaded \
    && helm init --client-only \
    && helm plugin install https://github.com/rimusz/helm-tiller \
    && helm repo add cloud-marketplace-tools-charts https://cloud-marketplace-tools.storage.googleapis.com/charts

COPY marketplace/deployer_util/* /bin/
COPY marketplace/deployer_helm_tiller_base/bin/* /bin/

RUN mkdir -p /data/chart

ENTRYPOINT ["/bin/bash", "/bin/deploy.sh"]
