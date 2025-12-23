# Devcontainer base image for Debian (glibc)
FROM mcr.microsoft.com/devcontainers/base:bookworm

# Install mongosh (MongoDB Shell) using the glibc-based tarball
RUN apt-get update && apt-get install -y --no-install-recommends curl ca-certificates && rm -rf /var/lib/apt/lists/* && \
    curl -o /tmp/mongosh.tgz https://downloads.mongodb.com/compass/mongosh-2.3.6-linux-x64.tgz && \
    tar -xzf /tmp/mongosh.tgz -C /tmp && \
    mv /tmp/mongosh-2.3.6-linux-x64/bin/* /usr/local/bin/ && \
    rm -rf /tmp/mongosh.tgz /tmp/mongosh-2.3.6-linux-x64 && \
    chmod +x /usr/local/bin/mongosh

# Auto-start mongosh when opening an interactive shell (set DISABLE_AUTO_MONGOSH=1 to skip)
RUN cat <<'EOF' >> /home/vscode/.bashrc
if [ -t 1 ] && [ -z "$DISABLE_AUTO_MONGOSH" ]; then
    if command -v mongosh >/dev/null 2>&1 && [ -n "$MONGO_INITDB_ROOT_USERNAME" ] && [ -n "$MONGO_INITDB_ROOT_PASSWORD" ]; then
        echo "Warte auf MongoDB für mongosh ..."
        for i in $(seq 1 30); do
            if mongosh --host mongo -u "$MONGO_INITDB_ROOT_USERNAME" -p "$MONGO_INITDB_ROOT_PASSWORD" --authenticationDatabase admin --eval "db.runCommand({ ping: 1 })" >/dev/null 2>&1; then
                break
            fi
            sleep 1
        done
        mongosh --host mongo -u "$MONGO_INITDB_ROOT_USERNAME" -p "$MONGO_INITDB_ROOT_PASSWORD" --authenticationDatabase admin
    fi
fi
EOF

# Use non-root user from base image
USER vscode
WORKDIR /workspaces/MongoDB
