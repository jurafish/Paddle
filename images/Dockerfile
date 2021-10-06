FROM ubuntu:20.04

ENV DEBIAN_FRONTEND noninteractive

RUN \
    apt-get -y update && \
    apt-get -y dist-upgrade && \
    apt-get -y install build-essential yasm nasm xxd pkgconf git curl wget cmake unzip subversion autoconf automake libtool autopoint cmake clang texinfo texi2html gperf gettext itstool ragel libc6-dev libssl-dev gtk-doc-tools gobject-introspection gawk meson ninja-build p7zip-full python3-distutils python3-apt python-is-python3 binutils-mingw-w64-i686 gcc-mingw-w64-x86-64 g++-mingw-w64-x86-64 gfortran-mingw-w64-x86-64 && \
    apt-get -y clean && \
    rm /usr/lib/gcc/*-w64-mingw32/*/libstdc++*.dll* && \
    rm /usr/lib/gcc/*-w64-mingw32/*/libgcc_s* && \
    rm /usr/lib/gcc/*-w64-mingw32/*/*.dll.a && \
    rm /usr/*-w64-mingw32/lib/*.dll.a && \
    mkdir /opt/ffbuild

ENV CARGO_HOME="/opt/cargo" RUSTUP_HOME="/opt/rustup" PATH="/opt/cargo/bin:${PATH}"
RUN curl https://sh.rustup.rs -sSf | bash -s -- -y --no-modify-path
RUN cargo install cargo-c && rm -rf "${CARGO_HOME}"/{registry,git}

ADD run_stage.sh /usr/bin/run_stage
ADD git-mini-clone.sh /usr/bin/git-mini-clone
ADD check-wget.sh /usr/bin/check-wget

RUN rustup target add x86_64-pc-windows-gnu
ADD toolchain.cmake /toolchain.cmake
ADD cross.meson /cross.meson

ENV FFBUILD_TARGET_FLAGS="--pkg-config=pkg-config --cross-prefix=x86_64-w64-mingw32- --arch=x86_64 --target-os=mingw32" \
    FFBUILD_TOOLCHAIN=x86_64-w64-mingw32 \
    FFBUILD_CROSS_PREFIX=x86_64-w64-mingw32- \
    FFBUILD_RUST_TARGET=x86_64-pc-windows-gnu \
    FFBUILD_PREFIX=/opt/ffbuild \
    FFBUILD_CMAKE_TOOLCHAIN=/toolchain.cmake \
    PKG_CONFIG_LIBDIR=/opt/ffbuild/lib/pkgconfig \
    CFLAGS="-static-libgcc -static-libstdc++ -I/opt/ffbuild/include -O2 -pipe -D_FORTIFY_SOURCE=2 -fstack-protector-strong" \
    CXXFLAGS="-static-libgcc -static-libstdc++ -I/opt/ffbuild/include -O2 -pipe -D_FORTIFY_SOURCE=2 -fstack-protector-strong" \
    LDFLAGS="-static-libgcc -static-libstdc++ -L/opt/ffbuild/lib -O2 -pipe -fstack-protector-strong" \
    DLLTOOL="x86_64-w64-mingw32-dlltool"

RUN pip3 install numpy protobuf wheel setuptools
RUN ulimit -n 102400
RUN \
    cd /opt && \
    git clone https://github.com/PaddlePaddle/Paddle.git && \
    cd Paddle && \
    git checkout release/2.1
