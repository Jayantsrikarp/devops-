<<<<<<< HEAD

# 📘 Lab Question 2: Continuous Integration Using Jenkins for Student Portal

## 📝 Problem Statement

Set up **Continuous Integration (CI)** for a small student portal website hosted on **GitHub** using **Jenkins**.
Configure a Jenkins **Freestyle Job** that:

* Automatically pulls code from GitHub
* Triggers a build whenever code is pushed
* Demonstrates an automatic build after an HTML update

---

## ✅ Complete Solution (Ubuntu + Jenkins + GitHub)

---

## 🔹 Step 0: System Prerequisites

### 1. Update system

```bash
sudo apt update
```

### 2. Install Java (required for Jenkins)

```bash
sudo apt install openjdk-17-jdk -y
=======
Alright. Another “sounds big, is actually mechanical” task.
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
>>>>>>> 414fba3 (Add README)
```

Verify:

```bash
<<<<<<< HEAD
java -version
=======
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
>>>>>>> 414fba3 (Add README)
```

---

<<<<<<< HEAD
## 🔹 Step 1: Install Jenkins

### 1. Add Jenkins key and repository

```bash
curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee \
/usr/share/keyrings/jenkins-keyring.asc > /dev/null
```

```bash
echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
/etc/apt/sources.list.d/jenkins.list > /dev/null
```

### 2. Install Jenkins

```bash
sudo apt update
sudo apt install jenkins -y
```

### 3. Start Jenkins

```bash
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

Check status:

```bash
sudo systemctl status jenkins
```

---

## 🔹 Step 2: Access Jenkins Web Interface

Open browser and go to:

```
http://localhost:8080
```

### Get initial admin password:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

* Paste password
* Click **Install Suggested Plugins**
* Create admin user
* Finish setup

---

## 🔹 Step 3: Create Student Portal Website (GitHub Repo)

### 1. Create project directory

```bash
mkdir student-portal
cd student-portal
```

### 2. Create HTML file

```bash
nano index.html
```

Paste:

```html
<!DOCTYPE html>
<html>
<head>
    <title>Student Portal</title>
</head>
<body>
    <h1>Welcome to Student Portal</h1>
    <p>This is the department student portal.</p>
</body>
</html>
```

Save and exit.

---

## 🔹 Step 4: Push Project to GitHub

### 1. Initialize Git

```bash
git init
git branch -M main
```

### 2. Commit files

```bash
git add index.html
git commit -m "Initial student portal website"
```

### 3. Create GitHub repository

* Repository name: `student-portal`
* Public
* Do NOT add README

### 4. Link and push

```bash
git remote add origin https://github.com/<your-username>/student-portal.git
git push -u origin main
```

---

## 🔹 Step 5: Create Jenkins Freestyle Job

### 1. In Jenkins dashboard

* Click **New Item**
* Name: `student-portal-ci`
* Select **Freestyle project**
* Click **OK**

---

## 🔹 Step 6: Configure GitHub Integration

### Source Code Management

* Select **Git**
* Repository URL:

```
https://github.com/<your-username>/student-portal.git
```

* Branch:

```
*/main
```

---

## 🔹 Step 7: Enable Automatic Build Trigger

### Build Triggers

* ✅ Check **GitHub hook trigger for GITScm polling**

Click **Save**.

This tells Jenkins: “If GitHub sends an update, wake up and build.”

---

## 🔹 Step 8: Initial Build Test

Click **Build Now**.

Confirm:

* Green checkmark
* Console Output shows Git pull success

---

## 🔹 Step 9: Configure GitHub Webhook

### In GitHub repository:

1. Go to **Settings → Webhooks**
2. Click **Add webhook**
3. Payload URL:

```
http://<your-ip>:8080/github-webhook/
```

(If local machine, use `localhost` only for demos)

4. Content type: `application/json`
5. Select **Just the push event**
6. Click **Add webhook**

---

## 🔹 Step 10: Make HTML Change (Trigger CI)

Edit HTML:

```bash
nano index.html
```

Modify content:

```html
<p>This portal provides student academic updates.</p>
```

Save and exit.

---

## 🔹 Step 11: Push Change to GitHub

```bash
git add index.html
git commit -m "Updated student portal content"
git push origin main
```

---

## 🔹 Step 12: Verify Automatic Jenkins Build

Go to Jenkins dashboard:

* New build should appear **automatically**
* No manual click required

Click build → **Console Output**
You’ll see Jenkins pulling the latest commit.

That’s Continuous Integration. No drama.

=======
## 🔹 Step 12: Remove the Docker Image

```bash
docker rmi flask-web-app
```

Verify:

```bash
docker images
```

>>>>>>> 414fba3 (Add README)
---

## 🔹 Commands Demonstrated (As Required)

```bash
<<<<<<< HEAD
git init
git add
git commit
git push
```

Jenkins:

* Freestyle Job
* Git SCM
* Webhook-triggered build

=======
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
>>>>>>> 414fba3 (Add README)
