FROM jenkins/jenkins:lts

USER root

# Install dependencies
RUN apt-get update && \
    apt-get install -y docker.io && \
    apt-get clean && \
    rm -rf /var/lib/apt/lists/*

# Setup buildx
RUN docker context create --docker host=unix:///var/run/docker.sock main-context && \
    docker context use main-context && \
    docker buildx create --use main-context --name main-builder --config ~/.docker/buildkitd.toml && \
    docker buildx use main-builder && \
    docker buildx inspect --bootstrap

# Install dependencies to enable specific architectures
RUN docker run --privileged --rm tonistiigi/binfmt --install arm64,arm/v6,arm/v7

# Or enable all architectures
# RUN docker run --privileged --rm tonistiigi/binfmt --install all

# Set buildx as the default builder
RUN docker buildx install
