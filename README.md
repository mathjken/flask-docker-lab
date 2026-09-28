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
```

## Requirements

* Docker Desktop
* WSL 2 / Ubuntu
* Git

## Build the Docker Image

From the project directory:

```bash
docker build -t flask-docker-lab:1.2 .
```

## Run the Container

```bash
docker run -d --name flask-docker-container-1.2 -p 5000:5000 flask-docker-lab:1.2
```

The application is available on port `5000`.

Test it with:

```bash
curl http://localhost:5000
```

Expected response:

```text
Flask Docker Lab is running!
```

## Container Security and Reliability

The Docker image runs the Flask application as a non-root user:

```dockerfile
RUN useradd --create-home appuser
USER appuser
```

A Docker health check is also configured to monitor the Flask application:

```dockerfile
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 CMD python -c "import urllib.request; urllib.request.urlopen('http://127.0.0.1:5000/')"
```

The container was verified to run as `appuser` and Docker reported the container status as `healthy`.

## Container Management

Check running containers:

```bash
docker ps
```

Stop the container:

```bash
docker stop flask-docker-container-1.2
```

Start an existing container:

```bash
docker start flask-docker-container-1.2
```

View container logs:

```bash
docker logs flask-docker-container-1.2
```

Remove a stopped container:

```bash
docker rm flask-docker-container-1.2
```

## Dockerfile Optimisation

The Dockerfile uses `python:3.12-slim` as the base image to reduce image size compared with a full Python image.

Dependencies are copied and installed before the application code:

```dockerfile
COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt
COPY app.py .
```

This improves Docker layer caching because changes to `app.py` do not require the dependency installation layer to be rebuilt.

The `--no-cache-dir` option also prevents pip's package cache from being stored in the image.

The application is run as a non-root user for improved container security, and a health check is included for basic runtime monitoring.

## Docker Hub

The verified Docker image is published on Docker Hub:

**Repository:** https://hub.docker.com/r/mathjken/flask-docker-lab

Image:

```text
mathjken/flask-docker-lab:1.2
```

The image was successfully pushed to Docker Hub using:

```bash
docker push mathjken/flask-docker-lab:1.2
```

## Verification

The Docker image was successfully built and the Flask application was successfully accessed through the mapped host port.

The `1.2` container was verified to:

* Start successfully
* Respond to HTTP requests
* Run as the non-root `appuser`
* Report a `healthy` Docker health status
* Expose port `5000`

Screenshots of the Docker container tests are included in the `screenshots/` directory.
