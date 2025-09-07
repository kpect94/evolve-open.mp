FROM ubuntu:18.04

ENV DEBIAN_FRONTEND=noninteractive

RUN \
    # Добавляем архитектуру i386
    dpkg --add-architecture i386 && \
    \
    apt-get update && \
    apt-get install -y wget gnupg software-properties-common && \
    \
    # Добавляем репозиторий для Clang-10
    wget -O - https://apt.llvm.org/llvm-snapshot.gpg.key | apt-key add - && \
    echo "deb http://apt.llvm.org/bionic/ llvm-toolchain-bionic-10 main" > /etc/apt/sources.list.d/llvm.list && \
    \
    apt-get update && \
    apt-get install -y \
        ninja-build \
        clang-10 \
        libclang-10-dev \
        python3-pip \
        gcc-8-multilib \
        g++-8-multilib \
        libstdc++6:i386 \
        libc6:i386 \
    && \
    \
    # Устанавливаем CMake 3.20.6
    wget https://cmake.org/files/v3.20/cmake-3.20.6-linux-x86_64.tar.gz && \
    tar zxvf cmake-3.20.6-linux-x86_64.tar.gz && \
    mv cmake-3.20.6-linux-x86_64 /opt/cmake-3.20.6 && \
    ln -sf /opt/cmake-3.20.6/bin/cmake /usr/bin/cmake && \
    \
    # Создаем пользователя
    useradd -m user && \
    \
    # Очистка
    apt-get clean && \
    rm -rf /var/lib/apt/lists/* && \
    rm cmake-3.20.6-linux-x86_64.tar.gz

USER user
RUN pip3 install --user "conan==1.57.0"

ENV CC=/usr/bin/clang-10 \
    CXX=/usr/bin/clang++-10 \
    PATH=/home/user/.local/bin:${PATH}

WORKDIR /home/user
COPY docker-entrypoint.sh /
CMD ["/docker-entrypoint.sh"]