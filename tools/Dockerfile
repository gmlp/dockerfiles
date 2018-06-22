from softtekcoe/zsh:latest

LABEL Maintainer="gmlp.24a@gmail.com"

USER root

RUN cd /tmp \
     && wget "https://releases.hashicorp.com/terraform/0.11.7/terraform_0.11.7_linux_amd64.zip?_ga=2.50275106.520816620.1523896114-1675264641.1505340345" -O terraform.zip \
     && wget "https://releases.hashicorp.com/packer/1.2.2/packer_1.2.2_linux_amd64.zip" \
     && wget "https://github.com/digitalocean/doctl/releases/download/v1.8.0/doctl-1.8.0-linux-amd64.tar.gz" \
     && wget https://storage.googleapis.com/kubernetes-helm/helm-v2.9.1-linux-amd64.tar.gz \
     && wget -O kops https://github.com/kubernetes/kops/releases/download/$(curl -s https://api.github.com/repos/kubernetes/kops/releases/latest | grep tag_name | cut -d '"' -f 4)/kops-linux-amd64 \
     && unzip terraform.zip \
     && unzip packer_*.zip \
     && tar -xzvf doctl-*.tar.gz \
     && tar -xzvf helm-*.tar.gz \
     && mv doctl /usr/local/bin/ \
     && mv kops /usr/local/bin/ \
     && mv linux-amd64/helm /usr/local/bin/ \
     && chmod +x /usr/local/bin/kops  \
     && mv packer /usr/local/bin \
     && mv terraform /usr/local/bin \
     && pip3 install awscli \
     && curl -L https://github.com/docker/compose/releases/download/1.21.0/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose \
     && chmod +x /usr/local/bin/docker-compose \
     && curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add - \
     && echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" > /etc/apt/sources.list.d/kubernetes.list \
     && apt-get update \
     && apt-get install -y \
        kubectl \
        groff \
        jq \
     && rm -rf /tmp/*

WORKDIR /home/user/projects

USER user

