FROM rust:1-slim-bullseye

ARG DEBIAN_FRONTEND=noninteractive

RUN rustup toolchain install nightly-2023-02-01-x86_64-unknown-linux-gnu
RUN rustup component add rustfmt clippy --toolchain nightly-2023-02-01-x86_64-unknown-linux-gnu
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        build-essential pkg-config git vim \
        libssl-dev ca-certificates apt-transport-https libclang-dev \
        python3-dev cmake protobuf-compiler gnupg python3-pip \
        curl

RUN echo "deb [arch=amd64] https://internal-archive.furiosa.dev/ubuntu focal restricted" \
        > /etc/apt/sources.list.d/furiosa.list && \
    echo "deb [arch=amd64] https://internal-archive.furiosa.dev/ubuntu focal-rc restricted" \
        >> /etc/apt/sources.list.d/furiosa.list && \
    echo "deb [arch=amd64] https://internal-archive.furiosa.dev/ubuntu focal-nightly restricted" \
        >> /etc/apt/sources.list.d/furiosa.list

ARG TOOLCHAIN_VERSION=0.9.0-2
ARG ONNXRUNTIME_VERSION=1.13.1-2

RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 5F03AFA423A751913F249259814F888B20B09A7E
RUN --mount=type=secret,id=furiosa.conf,dst=/etc/apt/auth.conf.d/furiosa.conf,required \
    APT_KEY_DONT_WARN_ON_DANGEROUS_USAGE=DontWarn \
    apt-get update && \
    apt-get install -y libonnxruntime=$ONNXRUNTIME_VERSION furiosa-libhal-warboy \
        furiosa-libcompiler=$TOOLCHAIN_VERSION \
        furiosa-libnux-extrinsic=$TOOLCHAIN_VERSION \
        furiosa-libnux=$TOOLCHAIN_VERSION
