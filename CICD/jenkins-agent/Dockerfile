FROM maven:3.9.11-eclipse-temurin-17

USER root

RUN sed -i 's|deb.debian.org|mirrors.aliyun.com|g' /etc/apt/sources.list \
    && sed -i 's|security.debian.org|mirrors.aliyun.com|g' /etc/apt/sources.list \
    && apt-get update \
    && apt-get install -y docker.io \
    && rm -rf /var/lib/apt/lists/*


# 安装 kubectl
RUN curl --proxy http://192.168.0.197:7897 \
    -LO https://dl.k8s.io/release/v1.35.7/bin/linux/amd64/kubectl \
    && install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl \
    && rm kubectl


# 安装 helm
RUN export HTTPS_PROXY=http://192.168.0.197:7897 \
    && export HTTP_PROXY=http://192.168.0.197:7897 \
    && curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash
