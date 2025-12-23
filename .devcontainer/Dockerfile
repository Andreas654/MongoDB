# Devcontainer base image for Alpine Linux
FROM mcr.microsoft.com/devcontainers/base:alpine

# Install mongosh (MongoDB Shell) for Alpine
RUN apk add --no-cache bash curl && \
    curl -o mongosh.tgz https://downloads.mongodb.com/compass/mongosh-2.3.6-linux-x64.tgz && \
    tar -xzf mongosh.tgz && \
    mv mongosh-2.3.6-linux-x64/bin/* /usr/local/bin/ && \
    rm -rf mongosh.tgz mongosh-2.3.6-linux-x64 && \
    chmod +x /usr/local/bin/mongosh

# Use non-root user from base image
USER vscode
WORKDIR /workspaces/MongoDB
