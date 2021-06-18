FROM python:3.9.5-slim

WORKDIR /tmp

RUN apt-get update \
    && apt-get install -y --no-install-recommends \
    make \
    wget \
    curl \
    less \
    git \
    zsh \
    vim \
    sudo \
    sshpass \
    git-extras \
    openssh-client \
    && rm -rf /var/lib/apt/lists/* \
    && rm -Rf /usr/share/doc && rm -Rf /usr/share/man \
    && apt-get clean

RUN useradd -md /home/avd -s /bin/zsh -u 1000 avd
USER avd

ENV PATH=$PATH:/home/avd/.local/bin

RUN pip3 install --upgrade pip
RUN pip3 install --user netaddr==0.7.19
RUN pip3 install --user Jinja2==2.11.3
RUN pip3 install --user treelib==1.5.5
RUN pip3 install --user cvprac==1.0.5
RUN pip3 install --user paramiko==2.7.1
RUN pip3 install --user jsonschema==3.2.0
RUN pip3 install --user requests==2.25.1
RUN pip3 install --user PyYAML==5.4.1
RUN pip3 install --user md-toc==7.1.0
RUN pip3 install --user "ansible>=2.10,<2.11"
RUN pip3 install --user ansible-lint
RUN pip3 install --user "galaxy-importer>=0.3.1,<0.3.2"
RUN pip3 install --user pycodestyle
RUN pip3 install --user flake8
RUN pip3 install --user pylint==2.4.4
RUN pip3 install --user twine
RUN pip3 install --user pre-commit==2.9.2
RUN pip3 install --user pre-commit-hooks==3.3.0
RUN pip3 install --user identify==1.4.20
RUN pip3 install --user docker
RUN pip3 install --user molecule==3.2.0
RUN pip3 install --user molecule-docker==0.2.4

WORKDIR /avd_docker_collections
RUN git clone --depth 1 --branch=v2.2.0 https://github.com/aristanetworks/ansible-avd.git
RUN git clone --depth 1 --branch=v3.0.0 https://github.com/aristanetworks/ansible-cvp.git

ARG TERM=TERM=xterm-256color

ENV DEBIAN_FRONTEND noninteractive
ENV PROJECT_DIR /projects/

# Copy gitconfig file
COPY files/gitconfig /home/avd/.gitconfig

# Install Oh My ZSH to provide improved shell
RUN wget --quiet https://github.com/robbyrussell/oh-my-zsh/raw/master/tools/install.sh -O - | zsh || true \
    && echo 'plugins=(ansible common-aliases safe-paste git jsontools history git-extras)' >> $HOME/.zshrc \
    && echo 'eval `ssh-agent -s`' >> $HOME/.zshrc \
    && echo 'export TERM=xterm' >>  $HOME/.zshrc \
    && echo "export LC_ALL=C.UTF-8" >> $HOME/.zshrc \
    && echo "export LANG=C.UTF-8" >> $HOME/.zshrc \
    && echo 'export PATH=$PATH:/home/avd/.local/bin' >> $HOME/.zshrc

# Set default folder
WORKDIR /projects
# VOLUME ["/projects"]

# Use ZSH as default shell with default oh-my-zsh theme
USER root
COPY files/entrypoint.sh /bin/entrypoint.sh
RUN chmod +x /bin/entrypoint.sh
CMD [ "/bin/entrypoint.sh" ]

USER avd
