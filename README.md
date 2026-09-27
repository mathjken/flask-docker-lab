# Flask Docker Lab

A simple Flask web application packaged and run using Docker.

## Project Structure

```text
flask-docker-lab/
├── app.py
├── requirements.txt
├── Dockerfile
├── .gitignore
└── screenshots/

Requirements
Docker
WSL 2 / Ubuntu
Git
Build the Docker Image

From the project directory:

docker build -t flask-docker-lab:1.0 .
Run the Container
docker run -d --name flask-docker-container -p 5000:5000 flask-docker-lab:1.0

The application is available on port 5000.

Test it with:

curl http://localhost:5000

Expected response:

Flask Docker Lab is running!
Container Management

Check running containers:

docker ps

Stop the container:

docker stop flask-docker-container

Start an existing container:

docker start flask-docker-container

View container logs:

docker logs flask-docker-container

Remove a stopped container:

docker rm flask-docker-container
Dockerfile Optimisation

The Dockerfile uses python:3.12-slim as the base image to reduce the size compared with a full Python image.

Dependencies are copied and installed before the application code:

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY app.py .

This improves Docker layer caching because changes to app.py do not require the dependency installation layer to be rebuilt.

The --no-cache-dir option also prevents pip's package cache from being stored in the image.

Verification

The Docker image was successfully built and the Flask application was successfully accessed through the mapped host port.

The resulting image was approximately 198 MB in Docker's reported disk usage.

Screenshots of the Docker container test are included in the screenshots/ directory.
