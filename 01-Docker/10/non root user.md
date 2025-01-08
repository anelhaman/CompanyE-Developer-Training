## Use a Non-Root User to Run the Docker Daemon

### CLI Example (Using a Non-Root User)

#### Step 1: Create a non-root user
First, create a user with appropriate permissions to run Docker.

```bash
# Create a non-root user (e.g., "dockeruser")
sudo useradd -m dockeruser

# Add the user to the docker group so that it can run Docker commands
sudo usermod -aG docker dockeruser
```

#### Step 2: Log in as the non-root user
Next, switch to the non-root user to run Docker commands.

```bash
# Switch to the newly created user
sudo su - dockeruser
```

#### Step 3: Run Docker commands
Now, you can run Docker commands as a non-root user.

```bash
# Example of running Docker as the non-root user
docker info
```

In this setup, the non-root user (`dockeruser`) has the necessary permissions to manage Docker containers, as it is a member of the `docker` group.

### Dockerfile Example (Running Container as Non-Root User)

In a `Dockerfile`, you can specify a non-root user to run your container by using the `USER` directive. Here's an example:

#### Step 1: Create a Dockerfile
```Dockerfile
# Use an official base image (e.g., Ubuntu)
FROM ubuntu:20.04

# Create a non-root user and set a working directory
RUN useradd -m dockeruser && \
    mkdir /app && \
    chown dockeruser:dockeruser /app

# Switch to the non-root user
USER dockeruser

# Set the working directory for the non-root user
WORKDIR /app

# Copy files to the /app directory (optional)
COPY . /app

# Run the application as a non-root user
CMD ["echo", "Running as non-root user"]
```

#### Step 2: Build and run the Docker container

```bash
# Build the Docker image
docker build -t myapp .

# Run the Docker container
docker run myapp
```

### Explanation:

- **CLI**: You add a non-root user (`dockeruser`) to the `docker` group so it has permission to run Docker commands. You can then execute Docker commands using that user.
- **Dockerfile**: In the `Dockerfile`, the `USER` directive ensures that the container runs as the specified user (`dockeruser` in this case), not as the root user.

This approach adds an extra layer of security by running Docker containers as non-root users whenever possible, both on the host machine and inside the container itself.
