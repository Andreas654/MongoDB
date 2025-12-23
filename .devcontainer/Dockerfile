# Devcontainer base image
FROM mcr.microsoft.com/devcontainers/base:ubuntu

# Install prerequisites and mongosh (MongoDB Shell)
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update \
  && apt-get install -y --no-install-recommends curl ca-certificates gnupg \
  && curl -fsSL https://www.mongodb.org/static/pgp/server-8.0.asc | \
     gpg -o /usr/share/keyrings/mongodb-server-8.0.gpg --dearmor \
  && echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-8.0.gpg ] https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/8.0 multiverse" | \
     tee /etc/apt/sources.list.d/mongodb-org-8.0.list \
  && apt-get update \
  && apt-get install -y --no-install-recommends mongodb-mongosh \
  && apt-get clean -y && rm -rf /var/lib/apt/lists/*

# Use non-root user from base image
USER vscode
WORKDIR /workspaces/MongoDB
