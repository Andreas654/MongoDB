# Devcontainer base image
FROM mcr.microsoft.com/devcontainers/base:ubuntu

# Install prerequisites and mongosh (MongoDB Shell)
ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update \
  && apt-get install -y --no-install-recommends gnupg curl ca-certificates lsb-release \
  && curl -fsSL https://www.mongodb.org/static/pgp/server-6.0.asc | gpg --dearmor -o /usr/share/keyrings/mongodb.gpg \
  && echo "deb [signed-by=/usr/share/keyrings/mongodb.gpg] https://repo.mongodb.org/apt/ubuntu $(lsb_release -cs)/mongodb-org/6.0 multiverse" \
     | tee /etc/apt/sources.list.d/mongodb-org-6.0.list \
  && apt-get update \
  && apt-get install -y --no-install-recommends mongosh \
  && apt-get clean -y && rm -rf /var/lib/apt/lists/*

# Use non-root user from base image
USER vscode
WORKDIR /workspaces/MongoDB
