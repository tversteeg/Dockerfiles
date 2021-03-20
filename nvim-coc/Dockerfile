FROM debian:buster
LABEL maintainer tversteeg

# XDG base directory
ENV HOME=/home/dev
ENV XDG_CACHE_HOME=$HOME/.cache \
  XDG_CONFIG_DIRS=$HOME/etc/xdg \
  XDG_CONFIG_HOME=$HOME/.config \
  XDG_DATA_DIRS=/usr/local/share:/usr/share \
  XDG_DATA_HOME=$HOME/.local/share
ENV DEBIAN_FRONTEND=noninteractive

RUN set -x \
  && : "Install basic tools" \
  && apt-get update \
  && apt-get install -y --no-install-recommends \
    ca-certificates \
    curl \
    git \
    wget \
  && : "Clean" \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

RUN set -x \
  && : "Install python" \
  && apt-get update \
  && apt-get install -y --no-install-recommends \
    python \
    python-pip \
    python3-dev \
    python3-pip \
  && : "Ignore pip upgrade warning" \
  && pip install --no-cache setuptools \
  && pip3 install --no-cache setuptools \
  && : "Clean" \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

RUN set -x \
  && : "Install node.js" \
  && apt-get update \
  && apt-get install -y --no-install-recommends \
    npm \
  && npm -g install yarn \
  && : "Clean" \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

RUN set -x \
  && : "Create home directory for all user" \
  && mkdir -p /home/dev \
  && chmod -R 777 /home/dev

RUN set -x \
  && : "Install latest neovim" \
  && wget -q https://github.com/neovim/neovim/releases/download/nightly/nvim.appimage \
  && chmod u+x ./nvim.appimage \
  && ./nvim.appimage --appimage-extract \
  && mv ./squashfs-root /opt/nvim \
  && chmod -R +rx /opt/nvim \
  && ln -s /opt/nvim/usr/bin/nvim /usr/local/bin/nvim \
  && : "Install nvim tool" \
  && pip install --no-cache pynvim \
  && pip3 install --no-cache pynvim \
  && npm install -g neovim \
  && : "Clean" \
  && rm nvim.appimage

COPY init.lua $XDG_CONFIG_HOME/nvim/
RUN set -x \
  && : "Create XDG base direcotry" \
  && mkdir -p $XDG_CACHE_HOME \
  && mkdir -p $XDG_DATA_HOME \
  && mkdir -p $XDG_CONFIG_HOME/nvim \
  && : "Install coc.nvim plugin" \
  && mkdir -p $XDG_DATA_HOME/nvim/site/pack/coc/start \
  && cd $XDG_DATA_HOME/nvim/site/pack/coc/start \
  && curl --fail -L https://github.com/neoclide/coc.nvim/archive/release.tar.gz|tar xzfv - \
  && cd coc* \
  && yarn install --frozen-lockfile \
  && : "Install registers.nvim plugin" \
  && git clone https://github.com/tversteeg/registers.nvim $XDG_DATA_HOME/nvim/site/pack/registers/start/registers

ENV DEBIAN_FRONTEND=dialog \
  SHELL=/bin/bash
CMD ["nvim"]
