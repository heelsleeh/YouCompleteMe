FROM ubuntu:18.04

ENV DEBIAN_FRONTEND=noninteractive
ENV LC_ALL C.UTF-8

ARG VIM_VERSION=v8.1.1875
ARG YCM_VIM_PYTHON=python3
ARG YCM_PYTHON_VERSION=3.5.2

RUN apt-get update && \
  apt-get -y dist-upgrade && \
  apt-get -y install ca-cacert \
                     locales \
                     tzdata \
                     language-pack-en \
                     libncurses5-dev libncursesw5-dev \
                     git \
                     build-essential \
                     cmake \
                     curl \
                     sudo \
                     python-dev \
                     python-pip \
                     python3-dev \
                     python3-pip \
                     zlib1g-dev && \
  apt-get -y autoremove

RUN ln -fs /usr/share/zoneinfo/Europe/London /etc/localtime && \
  dpkg-reconfigure --frontend noninteractive tzdata

ENV CONF_ARGS "--with-features=huge \
               --enable-${YCM_VIM_PYTHON}interp \
               --enable-terminal \
               --enable-multibyte \
               --enable-fail-if-missing"

RUN mkdir -p $HOME/vim && \
    cd $HOME/vim && \
    git clone https://github.com/vim/vim && \
    cd vim && \
    git checkout ${VIM_VERSION} && \
    make -j 4 && \
    make install

# linuxbrew (homebrew)
RUN mkdir -p /home/linuxbrew/.linuxbrew &&\
    chmod -R go+rwx /home/linuxbrew && \
    mkdir -p /home/linuxbrew/.linuxbrew/bin && \
    git clone  https://github.com/Homebrew/brew /home/linuxbrew/.linuxbrew/Homebrew && \
    ln -s /home/linuxbrew/.linuxbrew/Homebrew/bin/brew /home/linuxbrew/.linuxbrew/bin && \
    echo "eval \$(/home/linuxbrew/.linuxbrew/bin/brew shellenv)" \
        > /etc/bash.bashrc

# clean up
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* &&\
    /home/linuxbrew/.linuxbrew/bin/brew cleanup && \
    rm -rf ~/.cache && \
    rm -rf $HOME/vim
