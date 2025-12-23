# MongoDB Devcontainer

This repository provides a complete development environment for MongoDB using GitHub Codespaces or VS Code Remote - Containers (Dev Containers). The setup includes a MongoDB 6.0 server and mongosh (MongoDB Shell) pre-configured and ready to use.

## Quickstart

### Using GitHub Codespaces

1. Click the **Code** button on the repository page
2. Select **Create codespace on add-devcontainer-mongodb** (or your branch)
3. Wait for the codespace to build and start
4. MongoDB will start automatically in the background
5. Once ready, you'll see a message in the terminal with connection instructions

### Using VS Code Remote - Containers (Local)

1. Clone this repository:
   ```bash
   git clone https://github.com/Andreas654/MongoDB.git
   cd MongoDB
   ```

2. Copy the example environment file:
   ```bash
   cp .env.example .env
   ```

3. (Optional) Edit `.env` to customize MongoDB credentials:
   ```bash
   # Default values in .env.example:
   MONGO_INITDB_ROOT_USERNAME=root
   MONGO_INITDB_ROOT_PASSWORD=example
   ```

4. Open the folder in VS Code:
   ```bash
   code .
   ```

5. When prompted, click **Reopen in Container** (or run the command **Dev Containers: Reopen in Container** from the Command Palette)

6. Wait for the container to build and MongoDB to start

## Starting MongoDB

MongoDB starts automatically when the devcontainer launches. The `docker-compose.yml` configuration includes:

- **Service**: `mongo` running MongoDB 6.0
- **Port**: 27017 (exposed and forwarded)
- **Credentials**: Set via `.env` file (see `.env.example`)
- **Data persistence**: Volume `mongo_data` mounted at `/data/db`
- **Health check**: Automatic health monitoring using `mongosh`

The workspace container depends on MongoDB being healthy, so you can start using it immediately after the container is ready.

## Using mongosh (MongoDB Shell)

### Connect to MongoDB

Once inside the devcontainer, connect to MongoDB using:

```bash
mongosh --host mongo -u $MONGO_INITDB_ROOT_USERNAME -p $MONGO_INITDB_ROOT_PASSWORD --authenticationDatabase admin
```

The environment variables are automatically loaded from your `.env` file.

### Example Commands

After connecting with mongosh, try these commands:

```javascript
// Show all databases
show dbs

// Create or switch to a database
use mydb

// Insert a document
db.users.insertOne({ name: "Alice", age: 30 })

// Find documents
db.users.find()

// Exit mongosh
exit
```

## Troubleshooting

### MongoDB not connecting

1. **Check if MongoDB is running**:
   ```bash
   docker ps
   ```
   You should see both `mongo` and `workspace` containers running.

2. **Check MongoDB logs**:
   ```bash
   docker logs mongodb-mongo-1
   ```

3. **Verify MongoDB health**:
   ```bash
   docker inspect --format='{{.State.Health.Status}}' mongodb-mongo-1
   ```
   Should show `healthy`.

4. **Restart the containers**:
   From the VS Code Command Palette, run **Dev Containers: Rebuild Container**.

### Connection refused errors

- Ensure you're using `--host mongo` (the service name) not `localhost` when connecting from the workspace container
- Verify port 27017 is forwarded in the Ports tab (VS Code automatically forwards it based on devcontainer.json)

### Environment variables not loaded

- Ensure `.env` file exists in the repository root (copy from `.env.example`)
- Restart the devcontainer after creating/modifying `.env`

### Permission errors

- The workspace runs as the `vscode` user (non-root)
- MongoDB runs as the default MongoDB user inside its container
- If you encounter permission issues with mounted volumes, check Docker volume permissions

## Security Notes

### Credentials Management

- **Never commit `.env` to version control** - it contains sensitive credentials
- The `.env.example` file is provided as a template with placeholder values
- In production or shared environments, use stronger passwords than the examples
- For production use, consider using Docker secrets or a secrets management service

### Network Security

- By default, MongoDB is only accessible within the Docker network and on localhost
- Port 27017 is exposed for development convenience
- In production deployments, restrict network access using firewalls and authentication

### Authentication

- The setup uses MongoDB's built-in authentication with root credentials
- Always use authentication in development and production environments
- Consider creating application-specific users with limited permissions instead of using the root account

## Files Overview

- **docker-compose.yml**: Orchestrates MongoDB and workspace services
- **.env.example**: Template for environment variables (copy to `.env`)
- **.devcontainer/devcontainer.json**: Dev Container configuration for VS Code
- **.devcontainer/Dockerfile**: Custom image with mongosh pre-installed
- **README.md**: This documentation

## Additional Resources

- [MongoDB Documentation](https://docs.mongodb.com/)
- [mongosh Documentation](https://docs.mongodb.com/mongodb-shell/)
- [VS Code Dev Containers](https://code.visualstudio.com/docs/devcontainers/containers)
- [GitHub Codespaces](https://github.com/features/codespaces)