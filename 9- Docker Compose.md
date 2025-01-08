Docker Compose is a tool for defining and managing multi-container Docker applications. It uses a YAML file (`docker-compose.yml`) to configure the application's services, networks, and volumes, simplifying the orchestration of containers.


### project that use docker-compose

#### Step-by-Step Explanation

This project sets up a Python Flask application that uses Redis to count the number of visits to a web page. The setup involves Dockerizing the app and using Docker Compose to orchestrate the services.

#### 1. **Python Flask App**

- **Purpose**: A web app increments a counter every time the home page (`/`) is visited.

```python
import time
import redis
from flask import Flask

app = Flask(__name__)
cache = redis.Redis(host='redis', port=6379)  # Connect to the Redis container

# Function to increment the hit counter
def get_hit_count():
    retries = 5
    while True:
        try:
            return cache.incr('hits')  # Increment the Redis counter
        except redis.exceptions.ConnectionError as exc:
            if retries == 0:
                raise exc
            retries -= 1
            time.sleep(0.5)  # Retry if connection fails

@app.route('/')
def home():
    count = get_hit_count()
    return f"What's up Docker Deep Divers! You've visited me {count} times."

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```


#### 2. **Dockerfile**

- **Purpose**: Creates a Docker image for the Flask app.
```dockerfile
FROM python:3.6-alpine
ADD . /code
WORKDIR /code
RUN pip install -r requirements.txt
CMD ["python", "flask_app.py"]
```

- **FROM**: Base image is Python 3.6 Alpine (lightweight image).
- **ADD**: Copies the project files to `/code` in the image.
- **WORKDIR**: Sets the working directory inside the container.
- **RUN**: Installs dependencies from `requirements.txt`.
- **CMD**: Specifies the command to run the Flask app.

#### 3. **Docker Compose**

- **Purpose**: Orchestrates the Flask app and Redis container.
```yaml
version: "3.7"

services:
  web-fe:
    build: .
    command: python flask_app.py
    container_name: frontend_service
    ports:
      - "5000:5000"
    networks:
      - frontend-network
      - backend-network
    volumes:
      - counter-volume:/code

  redis:
    image: "redis:alpine"
    container_name: backend_service
    networks:
      - backend-network

networks:
  frontend-network:
  backend-network:
    internal: true

volumes:
  counter-volume:

```

- **web-fe**:
    
    - Builds the Flask app from the `Dockerfile`.
    - Maps port `5000` of the container to port `5000` on the host.
    - Shares networks (`frontend-network` and `backend-network`).
    - Uses a volume (`counter-volume`) for persistent storage.
- **redis**:
    
    - Uses the Redis Alpine image.
    - Attached only to `backend-network` for isolation.
- **networks**:
    
    - Two networks:
        - `frontend-network`: Connects services that need public access.
        - `backend-network`: Isolated network for Redis and Flask app.
- **volumes**:
    
    - `counter-volume` is used for persistent storage of the app's files.

#### 4. **Requirements File**

- **Purpose**: Specifies Python dependencies.
```plaintext
flask
redis
```

### **How to Run**

1. **Install Docker and Docker Compose**.
2. **Create the Project Structure**:
```bash
/project
├── Dockerfile
├── docker-compose.yml
├── flask_app.py
├── requirements.txt
```
3. **Build and Start Services**:
```bash
docker-compose up --build
```
4. **Access the App**:
	- Open your browser and go to `http://localhost:5000`.

![screen](Docker/images/9.1.png)


![screen](Docker/images/9.2.png)


### **Why Use Docker Compose?**

1. **Multi-Container Orchestration**:
    
    - Simplifies managing multiple services (Flask app and Redis).
    - Automatically creates necessary networks and volumes.
2. **Service Dependency**:
    
    - Ensures Redis is up and running before the Flask app starts (`depends_on` can be used for more control).
3. **Portability**:
    
    - Share the `docker-compose.yml` file to replicate the entire environment.
4. **Networking**:
    
    - Provides seamless communication between containers using service names (e.g., `redis` instead of hardcoded IPs).



