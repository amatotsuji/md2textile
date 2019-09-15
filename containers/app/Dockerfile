FROM ubuntu:disco

RUN apt-get update
RUN apt-get upgrade -y

# dependencies for rbenv and ruby
RUN apt-get install -y \
    git \
    build-essential \
    libssl-dev \
    libreadline-dev \
    zlib1g-dev \
		curl

RUN git clone https://github.com/rbenv/rbenv.git ~/.rbenv && \
    echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> ~/.bashrc && \
    echo 'eval "$(rbenv init -)"' >> ~/.bashrc

ENV PATH $HOME/.rbenv/shims:$HOME/.rbenv/bin:$PATH

RUN git clone https://github.com/rbenv/ruby-build.git ~/.rbenv/plugins/ruby-build
RUN ~/.rbenv/bin/rbenv install 2.5.0
RUN ~/.rbenv/bin/rbenv global 2.5.0

ENV RUBYOPT -EUTF-8

RUN ~/.rbenv/bin/rbenv exec gem install bundler -v 2.0.2

# preparations for rails
# RUN apt-get install -y libsqlite3-dev
ENV DEBIAN_FRONTEND=noninteractive
RUN apt-get install -y \
    tzdata
RUN curl -L git.io/nodebrew | perl - setup && \
    echo 'export PATH=$HOME/.nodebrew/current/bin:$PATH' >> ~/.bashrc
ENV PATH $HOME/.nodebrew/current/bin:$PATH
RUN ~/.nodebrew/current/bin/nodebrew install-binary v12.10.0 && \
		~/.nodebrew/current/bin/nodebrew use v12.10.0

RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list && \
    apt-get update && apt-get install -y yarn


# install gcloud commands
RUN apt-get install -y lsb-release
RUN export CLOUD_SDK_REPO="cloud-sdk-$(lsb_release -c -s)" && \
    echo "deb http://packages.cloud.google.com/apt $CLOUD_SDK_REPO main" | tee -a /etc/apt/sources.list.d/google-cloud-sdk.list && \
    curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add - && \
    apt-get update && apt-get install -y google-cloud-sdk

