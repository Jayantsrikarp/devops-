vAlright. Another “sounds big, is actually mechanical” task.
Below is a **README-style HOW-TO**, written so you can **execute without thinking** on **Ubuntu terminal only**. No gaps. No assumptions. No “you should know this”.

---

# 📘 Lab Task: Dockerizing a Simple Web Application (Step-by-Step HOW-TO)

## 📝 Problem Statement

Create a simple web application (using **HTML or Python Flask**), write a **Dockerfile**, build a **Docker image**, and run the container so the application is accessible in a browser using **port mapping**.

Also demonstrate Docker commands to:

* List images and containers
* Stop a running container
* Remove a container
* Remove an image

Commands to be shown:
`docker ps`, `docker stop`, `docker rm`, `docker rmi`

---

## ✅ Complete Solution (Ubuntu Terminal Only)

We will use a **Python Flask** app because it clearly shows port mapping and container behavior.

---

## 🔹 Step 0: Prerequisites (Do Not Skip)

### Check Docker installation

```bash
docker --version
```

If Docker is **not installed**:

```bash
sudo apt update
sudo apt install docker.io -y
```

Start Docker and enable it:

```bash
sudo systemctl start docker
sudo systemctl enable docker
```

Add your user to Docker group (avoids typing sudo every time):

```bash
sudo usermod -aG docker $USER
```

⚠️ **Log out and log back in** after this step.

---

## 🔹 Step 1: Create Project Directory

```bash
mkdir docker-web-app
cd docker-web-app
```

This directory will contain:

* Flask app
* requirements file
* Dockerfile

---

## 🔹 Step 2: Create Application Files

### Create files

```bash
touch app.py requirements.txt Dockerfile
```

---

## 🔹 Step 3: Write Flask Application

Open `app.py`:

```bash
nano app.py
```

Paste **exactly this**:

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    return "Hello from Docker!"

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=5000)
```

Save and exit:

* Ctrl + O → Enter
* Ctrl + X

---

## 🔹 Step 4: Add Dependencies

Open `requirements.txt`:

```bash
nano requirements.txt
```

Paste:

```
flask
```

Save and exit.

---

## 🔹 Step 5: Write Dockerfile

Open Dockerfile:

```bash
nano Dockerfile
```

Paste:

```Dockerfile
FROM python:3.10-slim

WORKDIR /app

COPY requirements.txt .
RUN pip install --no-cache-dir -r requirements.txt

COPY app.py .

EXPOSE 5000

CMD ["python", "app.py"]
```

Save and exit.

---

## 🔹 Step 6: Build Docker Image

Run this **inside the project directory**:

```bash
docker build -t flask-web-app .
```

Explanation:

* `-t flask-web-app` → image name
* `.` → current directory as build context

Verify image creation:

```bash
docker images
```

---

## 🔹 Step 7: Run Docker Container (Port Mapping)

```bash
docker run -d -p 5000:5000 --name flask-container flask-web-app
```

Explanation:

* `-d` → detached mode
* `-p 5000:5000` → host port → container port
* `--name flask-container` → container name

---

## 🔹 Step 8: Verify Application in Browser

Open browser and visit:

```
http://localhost:5000
```

Expected output:

```
Hello from Docker!
```

If you see this, the container is running correctly.

---

## 🔹 Step 9: List Containers and Images

### List running containers

```bash
docker ps
```

### List all containers

```bash
docker ps -a
```

### List Docker images

```bash
docker images
```

---

## 🔹 Step 10: Stop the Running Container

```bash
docker stop flask-container
```

Verify:

```bash
docker ps
```

(Container should not appear)

---

## 🔹 Step 11: Remove the Container

```bash
docker rm flask-container
```

Verify:

```bash
docker ps -a
```

---

## 🔹 Step 12: Remove the Docker Image

```bash
docker rmi flask-web-app
```

Verify:

```bash
docker images
```

---

## 🔹 Commands Demonstrated (As Required)

```bash
docker ps
docker ps -a
docker stop flask-container
docker rm flask-container
docker rmi flask-web-app
```

All required Docker operations have been executed and verified.

---

## 🧠 What You Achieved

* Created a Flask web application
* Dockerized the application using Dockerfile
* Built and ran a Docker image
* Accessed the app via browser using port mapping
* Managed containers and images using Docker CLI

In short: you turned code into a running service without touching a server. That’s the whole Docker sales pitch.

If there’s another question, send it. I’ll keep turning buzzwords into commands.

