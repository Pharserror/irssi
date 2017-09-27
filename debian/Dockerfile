FROM debian:stretch

RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential \
    ca-certificates \
    libdatetime-perl \
    libglib2.0-0 \
    libwww-perl \
    git \
    neovim \
    perl \
    tmux \
    wget \
    zsh \
    curl \
    python \
    python3 \
    perl \
    gnupg2 \
    gtk+3.0 \
    libwebkit2gtk-3.0 \
    webkitgtk+2 \
    webkitgtk+3.0 \
    libwebkitgtk-dev \
    libwebkitgtk-3.0-dev \
    sudo \
	&& rm -rf /var/lib/apt/lists/*

ENV HOME /home/user
RUN useradd --create-home --home-dir $HOME user \
	&& mkdir -p $HOME/.irssi \
	&& chown -R user:user $HOME \
    && mkdir -p /home/user/.emacs.d

RUN su - && adduser user sudo

RUN cd /home/user \
    && wget ftp://ftp.gnu.org/pub/gnu/emacs/emacs-25.2.tar.gz \
    && tar -xf emacs-25.2.tar.gz \ 
    && cd /home/user/emacs-25.2/ \
    && sudo apt-get build-dep -y emacs25 \ 
    && sudo ./configure --with-cairo --with-xwidgets --with-x-toolkit=gtk3 \
    && sudo make \
    && sudo make install

RUN mkdir /home/vagrant/.config \
    && sudo chown -R vagrant /home/vagrant/.config \
    && curl -sLf https://spacevim.org/install.sh > /home/user/install.sh \
    && sudo chown user install.sh \
    && sudo chmod +x install.sh \
    && runuser -l vagrant -c 'sh /home/user/install.sh'

ENV LANG C.UTF-8

ENV IRSSI_VERSION 1.0.4

RUN buildDeps=' \
		autoconf \
		automake \
		bzip2 \
		dpkg-dev \
        gnupg \
		libglib2.0-dev \
		libncurses-dev \
		libperl-dev \
		libssl-dev \
		libtool \
		lynx \
		make \
        neovim \
		pkg-config \
		xz-utils \
	' \
	&& set -x \
	&& apt-get update && apt-get install -y $buildDeps --no-install-recommends \
	&& rm -rf /var/lib/apt/lists/* \
	&& wget "https://github.com/irssi/irssi/releases/download/${IRSSI_VERSION}/irssi-${IRSSI_VERSION}.tar.xz" -O /tmp/irssi.tar.xz \
	&& wget "https://github.com/irssi/irssi/releases/download/${IRSSI_VERSION}/irssi-${IRSSI_VERSION}.tar.xz.asc" -O /tmp/irssi.tar.xz.asc \
	&& export GNUPGHOME="$(mktemp -d)" \
# gpg: key DDBEF0E1: public key "The Irssi project <staff@irssi.org>" imported
	&& gpg --keyserver ha.pool.sks-keyservers.net --recv-keys 7EE65E3082A5FB06AC7C368D00CCB587DDBEF0E1 \
	&& gpg --batch --verify /tmp/irssi.tar.xz.asc /tmp/irssi.tar.xz \
	&& rm -rf "$GNUPGHOME" /tmp/irssi.tar.xz.asc \
	&& mkdir -p /usr/src/irssi \
	&& tar -xf /tmp/irssi.tar.xz -C /usr/src/irssi --strip-components 1 \
	&& rm /tmp/irssi.tar.xz \
	&& cd /usr/src/irssi \
	&& gnuArch="$(dpkg-architecture --query DEB_BUILD_GNU_TYPE)" \
	&& ./configure \
		--build="$gnuArch" \
		--enable-true-color \
		--with-bot \
		--with-proxy \
		--with-socks \
	&& make -j "$(nproc)" \
	&& make install \
	&& rm -rf /usr/src/irssi \
	&& apt-get purge -y --auto-remove $buildDeps

WORKDIR $HOME

USER user
CMD ["nvim"]
