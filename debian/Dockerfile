FROM debian:stretch

RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    ca-certificates \
    libdatetime-perl \
    libglib2.0-0 \
    libwww-perl \
    git \
    perl \
    tmux \
    wget \
    zsh \
    curl \
    python \
    python3 \
    perl \
    gnupg2 \
    emacs25 \
    autoconf \
    automake \
    bzip2 \
    dpkg-dev \
    libglib2.0-dev \
    libncurses-dev \
    libperl-dev \
    libssl-dev \
    libtool \
    lynx \
    make \
    pkg-config \
    xz-utils \
    irssi \
    && rm -rf /var/lib/apt/lists/*

ENV HOME /home/user
RUN useradd --create-home --home-dir $HOME user \
	&& mkdir -p $HOME/.irssi \
	&& chown -R user:user $HOME \
    && mkdir -p /home/user/.emacs.d

RUN git clone https://github.com/syl20bnr/spacemacs /home/user/.emacs.d \
    && chown -R user /home/user/.emacs.d \
    && mkdir /home/user/.emacs.d/.cache \
    && chown -R user /home/user/.emacs.d/.cache

WORKDIR $HOME

USER user
CMD ["emacs"]
