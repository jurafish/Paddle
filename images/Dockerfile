FROM ubuntu:20.04

ENV DEBIAN_FRONTEND noninteractive

RUN \
    apt-get -y update && \
    apt-get -y dist-upgrade && \
    apt-get -y install gcc g++ make cmake git vim unrar python3 python3-dev python3-pip swig wget patchelf libopencv-dev  binutils-mingw-w64-i686 gcc-mingw-w64-x86-64 g++-mingw-w64-x86-64 gfortran-mingw-w64-x86-64 && \
    apt-get -y clean && \
    rm /usr/lib/gcc/*-w64-mingw32/*/libstdc++*.dll* && \
    rm /usr/lib/gcc/*-w64-mingw32/*/libgcc_s* && \
    rm /usr/lib/gcc/*-w64-mingw32/*/*.dll.a && \
    rm /usr/*-w64-mingw32/lib/*.dll.a

RUN pip3 install numpy protobuf wheel setuptools
RUN ulimit -n 102400
RUN \
    cd /opt && \
    git clone https://github.com/PaddlePaddle/Paddle.git && \
    cd Paddle && \
    git checkout release/2.1
