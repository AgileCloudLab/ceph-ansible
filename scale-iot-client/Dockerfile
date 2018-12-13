FROM ubuntu:18.04

ENV DEBIAN_FRONTEND noninteractive
# Install librados
RUN apt-get update && \
    apt-get install -y curl wget unzip gnupg software-properties-common && \
    wget -O - https://apt.llvm.org/llvm-snapshot.gpg.key | apt-key add - && \
    apt-add-repository "deb http://apt.llvm.org/xenial/ llvm-toolchain-xenial-6.0 main" && \
    apt-get update && \
    apt-get install -y clang-6.0 && \
    apt install -y postgresql postgresql-contrib libpqxx3-dev libpq-dev && \
    apt-get install -y build-essential librados-dev

# Download and install Cryptopp
RUN cd /tmp && \
    wget "https://www.cryptopp.com/cryptopp700.zip" && \
    unzip cryptopp700.zip -d cryptopp700 && \
    cd cryptopp700 && \
    export CXX=clang++-6.0 && \
    export CXXFLAGS="" && \
    make static dynamic cryptest.exe && \
    make libcryptopp.a libcryptopp.so cryptest.exe && \
    make install clean PREFIX=/usr/local

# Create infinite loop script to run container in daemon mode
RUN echo 'sleep infinity' >> /bootstrap.sh
RUN chmod +x /bootstrap.sh

CMD /bootstrap.sh
ENV DEBIAN_FRONTEND teletype
