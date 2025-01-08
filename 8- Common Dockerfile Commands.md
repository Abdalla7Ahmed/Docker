
#### 1. **FROM**

- **Purpose**: Specifies the base image for your new image.
- **Syntax and Example**:
```dockerfile
FROM <image>:<tag>
#############################
FROM python:3.9-slim
```

#### 2. **RUN**

- **Purpose**: Executes a command during the build process (e.g., install software or update packages).
-  **Syntax and Example**:
```dockerfile
RUN <command>
#############################
RUN apt-get update && apt-get install -y curl
```
#### 3. **CMD**

- **Purpose**: Specifies the default command to run when a container starts (can be overridden).
- **Syntax and Example**:
```dockerfile
CMD ["executable", "arg1", "arg2"]
#############################
CMD ["python", "app.py"]
```

#### 4. **ENTRYPOINT**


- **Purpose**: Similar to CMD but harder to override, typically used to define a fixed container behavior.
- **Syntax and Example**:
```dockerfile
ENTRYPOINT ["executable", "arg1"]
#############################
ENTRYPOINT ["nginx", "-g", "daemon off;"]
```

#### 5. **COPY**

- **Purpose**: Copies files or directories from the local machine to the image.
- **Syntax and Example**:
```dockerfile
COPY <source> <destination>
#############################
COPY requirements.txt /app/
```

#### 6. **ADD**

- **Purpose**: Similar to COPY but can also handle remote URLs and extract tar files.
- **Syntax and Example**:
```dockerfile
ADD <source> <destination>
#############################
ADD http://example.com/file.tar.gz /app/
```
#### 7. **WORKDIR**

- **Purpose**: Sets the working directory for subsequent instructions.
- if the directory doesn't exist, this will create the directory
- **Syntax and Example**:
```dockerfile
WORKDIR <path>
#############################
WORKDIR /app
```
#### 8. **ENV**

- **Purpose**: Sets environment variables in the image.
- **Syntax and Example**:
```dockerfile
ENV <key> <value>
#############################
ENV PORT 8080
```

#### 9. **EXPOSE**

- **Purpose**: Documents the ports the container listens on (does not publish the port).
- **Syntax and Example**:
```dockerfile
EXPOSE <port>
#############################
EXPOSE 80
```

#### 10. **VOLUME**

- **Purpose**: Defines mount points to persist data or share files between host and container.
- **Syntax and Example**:
```dockerfile
VOLUME ["<path>"]
#############################
VOLUME ["/data"]
```

#### 11. **ARG**

- **Purpose**: Defines build-time variables.
- **Syntax and Example**:
```dockerfile
ARG <name>[=<default>]
#############################
ARG VERSION=1.0
RUN echo $VERSION
```

#### 12. **LABEL**

- **Purpose**: Adds metadata to the image (e.g., version, maintainer).
- **Syntax and Example**:
```dockerfile
LABEL <key>=<value>
#############################
LABEL maintainer="you@example.com"
```

#### 13. **USER**

- **Purpose**: Sets the user for running the container.
- **Syntax and Example**:
```dockerfile
USER <username>
#############################
USER nonrootuser
```
#### 14. **ONBUILD**

- **Purpose**: Adds triggers to the image for use in child images.
- **Syntax and Example**:
```dockerfile
ONBUILD <instruction>
#############################
ONBUILD RUN echo "This runs in child images."
```

### Example Dockerfile
```dockerfile
# Set the base image
FROM python:3.9-slim

# Set environment variables
ENV APP_HOME=/app

# Set working directory
WORKDIR $APP_HOME

# Copy application files
COPY . $APP_HOME

# Install dependencies
RUN pip install -r requirements.txt

# Expose port
EXPOSE 8080

# Default command
CMD ["python", "app.py"]
```