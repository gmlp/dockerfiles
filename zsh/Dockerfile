FROM ubuntu:16.04

LABEL Maintainer="gmlp.24a@gmail.com"

RUN apt-get update \
 && apt-get install -y \
     tmux \
     git \
     curl \
     wget \
     vim \
     zsh \
     sudo \
     && mkdir /home/user \
     && git clone https://github.com/robbyrussell/oh-my-zsh.git -b master --single-branch --depth=1 /home/user/.oh-my-zsh \
     && wget https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/templates/zshrc.zsh-template -O /home/user/.zshrc \
     && useradd -d /home/user -s /bin/zsh user \
     && chown user:user -R /home/user \
     && su - user -c "sh <(curl https://j.mp/spf13-vim3 -L)" \
     && echo "user ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

RUN cd /tmp \
    && wget "https://download.docker.com/linux/static/stable/x86_64/docker-17.12.1-ce.tgz" \
    && tar -xzvf docker-* \
    && cd docker \
    && mv docker /usr/local/bin \
    && usermod -a user -G users \
    && rm -rf /tmp/*

WORKDIR /home/user/projects

USER user

