The process of taking an application and configuring it to run as a container is called “containerizing".

Containerizing an application involves packaging the application code, dependencies, and runtime environment into a Docker container. This ensures the app runs consistently across different environments.

The process of containerizing an app looks like this:

1. Start with your application code and dependencies
2. Create a _Dockerfile_ that describes your app, its dependencies, and how to run it
3. Feed the _Dockerfile_ into the `docker image build` command
4. Push the new image to a registry (optional)
5. Run container from the image


![screen](Docker/images/7.1.png)
 
### Steps to Containerize an App

**1. Write the Application**
Example: A simple Python web server using Flask. 
```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    return "Hello, Docker!"

@app.route('/test')
def test():
	return "A7la mesa 3alek !"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

This app listens on port `5000` and responds with "Hello, Docker!" for '/' and "A7la mesa 3alek !" for '/test'.

**2. Create the requirements.txt file

```plaintext
flask
```

**3. Create a Dockerfile**

```dockerfile
# Use an official Python runtime as a base image
FROM python

# Set the working directory in the container
WORKDIR /app

# Copy the application files
COPY flask_app.py requirements.txt /app/

# Install dependencies
RUN pip install -r requirements.txt

# Expose the port the app runs on
EXPOSE 5000

# Define the command to run the application
CMD ["python", "flask_app.py"]
```

**Explanation:**
- `FROM`: Specifies the base image (Python in this case).
- `WORKDIR`: Sets the working directory inside the container.
- `COPY`: Copies application files into the container.
- `RUN`: Installs dependencies (e.g., Flask).
- `EXPOSE`: Documents the port the container listens on.
- `CMD`: Specifies the command to start the application.


**4. Build the Docker Image**
Use the `docker build` command to create an image from the Dockerfile.
```bash
docker build -t my-flask-app .
```
**Explanation:**

- `-t my-flask-app`: Tags the image with a name (`my-flask-app`).
- `.`: Specifies the current directory as the build context.

![screen](Docker/images/7.2.png)


**5. Run the Docker Container**
Start a container from the built image.
```bash
docker run -d -p 5000:5000 --name flask-container my-flask-app
```

**Explanation:**

- `-d`: Runs the container in detached mode (background).
- `-p 5000:5000`: Maps port 5000 on the host to port 5000 in the container.
- `--name flask-container`: Assigns a name to the container.


**6. Test the Application**
Open a browser or use `curl` to verify the app is running:

![screen](Docker/images/7.3.png)

```bash
curl http://localhost:5000
curl http://localhost:5000/test
```

![screen](Docker/images/7.4.png)


**7. Push the Image to Docker Hub (Optional)**
To share the containerized app, push the image to Docker Hub:
```bash
docker login #login with you username and password
docker tag my-flask-app username/my-flask-app
docker push username/my-flask-app
```

![screen](Docker/images/7.5.png)

 
### Docker Image Layers

Docker images are built in a layered file system. Each instruction in a Dockerfile which change in the image filesystem, creates a new **layer**. Layers are stacked on top of each other to form the final image. This architecture allows for efficient storage and reuse.

### Key Concepts of Docker Layers

1. **Layering System**:
    - Each layer represents a change or modification in the image's filesystem, like adding files, installing software, or setting environment variables.
    - Layers are immutable and read-only.
2. **Base Layer**:
    - The starting point for an image, usually a lightweight operating system like `alpine` or `ubuntu`.
3. **Intermediate Layers**:
    - Created from Dockerfile instructions (`RUN`, `COPY`, etc.).
    - Each command generates a new layer that builds upon the previous one.
4. **Writable Layer**:
    - When a container is run from an image, Docker adds a **writable layer** on top of the read-only image layers.
    - All changes (new files, modifications) are stored in this layer and are ephemeral unless persisted.


```dockerfile
# Base layer
FROM python:3.9-slim

# New layer: create /app directory because it doesn't exist, then set the working directory /app
WORKDIR /app

# New layer: Copy the application files
COPY app.py requirements.txt /app/

# New layer: Install dependencies
RUN pip install -r requirements.txt

# Expose the port the app runs on(Updates metadata of the image)
EXPOSE 5000

# Define the command to run the application(Updates metadata of the image)
CMD ["python", "app.py"]
```

let's compare with the base image (python) and the new image (my-flask-app)

the layers of python image 

![screen](Docker/images/7.6.png)

the layers of my-flask-app image 

![screen](Docker/images/7.7.png)

if we compare the two images, three layers added to the my-flask-app image 
WORKDIR /app --> new layer
COPY flask_app.py requirements.txt /app/ --> new layer
RUN pip install -r requirements.txt --> new layer
![screen](Docker/images/7.8.png)