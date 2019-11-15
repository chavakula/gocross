# Golang 1.13.4 CGO based cross compiling with Goreleaser 
# Copyright (c) 2019 chavakula. All rights reserved
# MIT license

FROM ubuntu:16.04

MAINTAINER Rajshekar Chavakula <ch.rajshekar@gmail.com>

# Update apt-get & Install required packages to cross compile
RUN \
  apt-get update && \
  apt-get install -y automake autogen build-essential ca-certificates cmake              \
    gcc-5-arm-linux-gnueabi g++-5-arm-linux-gnueabi libc6-dev-armel-cross                \
    gcc-5-arm-linux-gnueabihf g++-5-arm-linux-gnueabihf libc6-dev-armhf-cross            \
    gcc-5-aarch64-linux-gnu g++-5-aarch64-linux-gnu libc6-dev-arm64-cross                \
    gcc-5-mips-linux-gnu g++-5-mips-linux-gnu libc6-dev-mips-cross  curl                 \
    gcc-5-mipsel-linux-gnu g++-5-mipsel-linux-gnu libc6-dev-mipsel-cross                 \
    gcc-5-mips64-linux-gnuabi64 g++-5-mips64-linux-gnuabi64 libc6-dev-mips64-cross       \
    gcc-5-mips64el-linux-gnuabi64 g++-5-mips64el-linux-gnuabi64 libc6-dev-mips64el-cross \
    gcc-5-multilib g++-5-multilib gcc-mingw-w64 g++-mingw-w64 clang llvm-dev             \
    libtool libxml2-dev uuid-dev libssl-dev swig openjdk-8-jdk pkg-config patch          \
    make xz-utils cpio wget zip unzip p7zip git mercurial bzr texinfo help2man           \
    --no-install-recommends

# Fix any stock package issues
RUN ln -s /usr/include/asm-generic /usr/include/asm

# Configure the container for OSX cross compilation
ENV OSX_NDK_X86 /usr/local/osx-ndk-x86
RUN wget -q --show-progress https://s3.dockerproject.org/darwin/v2/MacOSX10.11.sdk.tar.xz
RUN git clone https://github.com/tpoechtrager/osxcross.git
RUN mv MacOSX10.11.sdk.tar.xz /osxcross/tarballs/
RUN sed -i -e 's|-march=native||g' /osxcross/build_clang.sh /osxcross/wrapper/build_wrapper.sh
RUN UNATTENDED=yes OSX_VERSION_MIN=10.6 /osxcross/build.sh
RUN mv /osxcross/target $OSX_NDK_X86
RUN rm -rf /osxcross
ENV PATH $OSX_NDK_X86/bin:$PATH

# Install Golang1.13.4
RUN wget -q --show-progress https://dl.google.com/go/go1.13.4.linux-amd64.tar.gz
RUN tar -xf go1.13.4.linux-amd64.tar.gz
RUN mv go /usr/local
ENV GOROOT /usr/local/go
ENV PATH $GOROOT/bin:$PATH
RUN go version

# Install Dep
ENV GOBIN=/usr/bin
RUN curl https://raw.githubusercontent.com/golang/dep/master/install.sh | sh

# Install Goreleaser
RUN curl -sfL https://install.goreleaser.com/github.com/goreleaser/goreleaser.sh | sh
RUN mv ./bin/goreleaser /usr/bin/

# Enjoy go cross compiling!


