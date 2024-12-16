# Lab 9: Simple Web Server with Docker

This project demonstrates how to create a simple web server using Flask (Python) or Node.js (JavaScript), package it into a Docker image, and run it locally. Finally, the Docker image is tagged and pushed to Docker Hub.

## Table of Contents
- [Project Overview](#project-overview)
- [Prerequisites](#prerequisites)
- [Setup and Usage](#setup-and-usage)
  - [1. Clone Repository](#1-clone-repository)
  - [2. Build and Run the Web Server](#2-build-and-run-the-web-server)
  - [3. Build Docker Image](#3-build-docker-image)
  - [4. Run Docker Container Locally](#4-run-docker-container-locally)
  - [5. Tag and Push Docker Image to Docker Hub](#5-tag-and-push-docker-image-to-docker-hub)
- [Repository Structure](#repository-structure)
- [License](#license)

## Project Overview
This project creates a simple web server that responds with a message when accessed via a web browser or an HTTP client. The web server is containerized using Docker, enabling easy deployment and scaling.

Technologies used:
- **Flask** (Python-based framework) or **Node.js** (JavaScript-based runtime) for the web server.
- **Docker** for containerization.
- **Docker Hub** for hosting the Docker image.

## Prerequisites
Before you begin, ensure you have the following installed:
- [Docker](https://www.docker.com/) (version 20.10 or later)
- [Python 3.7+](https://www.python.org/) or [Node.js](https://nodejs.org/) (if running without Docker)
- A [Docker Hub](https://hub.docker.com/) account

## Setup and Usage

### 1. Clone Repository
```bash
git clone https://github.com/Kaydarkey/lab-9-simple-web-server--docker-image.git
cd lab-9-simple-web-server--docker-image
```

### 2. Build and Run the Web Server

#### If using Flask (Python):
1. Install dependencies:
   ```bash
   pip install -r requirements.txt
   ```
2. Run the server:
   ```bash
   python app.py
   ```
3. Access the server at [http://localhost:5000](http://localhost:5000).

#### If using Node.js:
1. Install dependencies:
   ```bash
   npm install
   ```
2. Run the server:
   ```bash
   node app.js
   ```
3. Access the server at [http://localhost:3000](http://localhost:3000).

### 3. Build Docker Image
Build the Docker image from the `Dockerfile`:
```bash
docker build -t simple-web-server:latest .
```

### 4. Run Docker Container Locally
Run the Docker container:
```bash
docker run -d -p 8080:5000 simple-web-server:latest
```
Access the server at [http://localhost:8080](http://localhost:8080).

### 5. Tag and Push Docker Image to Docker Hub
1. Tag the Docker image:
   ```bash
   docker tag simple-web-server:latest <your-dockerhub-username>/simple-web-server:latest
   ```
2. Log in to Docker Hub:
   ```bash
   docker login
   ```
3. Push the Docker image:
   ```bash
   docker push <your-dockerhub-username>/simple-web-server:latest
   ```

## Repository Structure
```
lab-9-simple-web-server--docker-image/
├── app.js                 # Node.js application (if using Node.js)
├── Dockerfile             # Dockerfile to build the image
├── requirements.txt       # Python dependencies (if using Flask)
├── package.json           # Node.js dependencies (if using Node.js)
├── README.md              # Project documentation
```

## License
This project is licensed under the MIT License. See the LICENSE file for more details.

---

