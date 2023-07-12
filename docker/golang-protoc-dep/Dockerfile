FROM ubuntu:16.04

WORKDIR /home

ENV GOVERSION 1.18
ENV GOROOT /usr/local/go
ENV GOPATH /go
ENV GOBIN /go/bin
ENV GO111MODULE on
ENV PATH ${GOBIN}:${GOROOT}/bin:${GOPATH}/bin:$PATH

RUN apt-get update

# install smc
RUN apt-get install -y python-pip docker.io
RUN pip install --upgrade "pip < 21.0"
RUN pip install -U setuptools
RUN curl -sSL http://deb.archive.garenanow.com/snippets/shopee_ubuntu_desktop_init.sh | sed '/ln/d' | bash

# install tools
RUN apt-get install -y apt-utils
RUN apt-get install -y zlib1g-dev 
RUN apt-get install -y autoconf automake libtool curl make g++ unzip git wget
RUN apt install -y vim

# install protobuf_cpp
RUN curl -LO https://github.com/protocolbuffers/protobuf/releases/download/v3.15.8/protobuf-cpp-3.15.8.tar.gz
RUN tar -xvf protobuf-cpp-3.15.8.tar.gz 
RUN cd protobuf-3.15.8 && ./autogen.sh && ./configure && make && make install && ldconfig
RUN ln -s /usr/local/bin/protoc /usr/local/bin/protoc3.2

#install golang
RUN cd /usr/local/ && wget https://storage.googleapis.com/golang/go${GOVERSION}.linux-amd64.tar.gz && \
    tar zxf go${GOVERSION}.linux-amd64.tar.gz && rm go${GOVERSION}.linux-amd64.tar.gz && \
    mkdir -p $GOPATH && mkdir -p $GOBIN

#install pkgconfig
RUN apt-get install -y pkg-config

# install dep
RUN curl https://raw.githubusercontent.com/golang/dep/master/install.sh | sh

# Copy over private key, and set permissions
# Warning! Anyone who gets their hands on this image will be able
# to retrieve this private key file from the corresponding image layer

# cleanup
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# docker image rm mygolang && docker build -f Dockerfile -t mygolang . 