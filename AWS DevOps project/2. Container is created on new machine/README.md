Yes, you can automate the setup process by creating a Dockerfile for your Jenkins container that includes these setup steps. This way, whenever the Jenkins container is created on a new machine or environment, these steps will be automatically executed during the container initialization.
Here's how you can create a Dockerfile for your Jenkins container:


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


In this Dockerfile:
• We start from the official Jenkins LTS image.
• We switch to the root user to perform installations and configurations.
• We install Docker CLI to interact with Docker daemon.
• We run the setup commands to configure buildx and enable architectures.
• Finally, we set buildx as the default builder.

Build the Docker image using the following command:
docker build -t my-jenkins .

Then, when you run the Jenkins container from this image, all the setup steps will be automatically executed:

docker run -d -p 8080:8080 -p 50000:50000 --name my-jenkins my-jenkins

This approach ensures that every time a new Jenkins container is created from the image, it will automatically perform the required setup steps.
