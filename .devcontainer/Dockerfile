# Devcontainer base image for Alpine Linux
FROM mcr.microsoft.com/devcontainers/base:alpine

# Install mongosh (MongoDB Shell) for Alpine
# gcompat/libc6-compat provide the glibc loader needed by the upstream mongosh binary
RUN apk add --no-cache bash curl gcompat libc6-compat && \
    curl -o mongosh.tgz https://downloads.mongodb.com/compass/mongosh-2.3.6-linux-x64.tgz && \
    tar -xzf mongosh.tgz && \
    mv mongosh-2.3.6-linux-x64/bin/* /usr/local/bin/ && \
    rm -rf mongosh.tgz mongosh-2.3.6-linux-x64 && \
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
