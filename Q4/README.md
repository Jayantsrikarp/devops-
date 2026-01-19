Alright. Now that we’ve **actually debugged reality** and not theory, here is **Question 4 done correctly**, cleanly, and in one place.

This is the **final, correct version of Q4 for Ubuntu using KIND**, written as a **proper HOW-TO**.
No Snap. No Minikube. No broken assumptions. This **will run**.

---

# 📘 Question 4: Deploy Docker Image on Kubernetes (Ubuntu – Correct Method)

## 📝 Problem Statement

Deploy a Dockerized web application on Kubernetes using a **Deployment YAML** file.
Verify that the pod is running using `kubectl` commands.
Expose the application using a **NodePort Service**.
Access it using the node’s IP and port.
Scale the deployment to **3 replicas** using `kubectl scale`.

---

## ✅ Solution (Ubuntu, KIND-based Kubernetes)

---

## 🔹 Step 0: Prerequisites

### Update system

```bash
sudo apt update
```

### Install Docker

```bash
sudo apt install docker.io -y
sudo systemctl start docker
sudo systemctl enable docker
```

Add user to Docker group:

```bash
sudo usermod -aG docker $USER
```

➡️ Log out and log back in.

Verify Docker:

```bash
docker ps
```

---

## 🔹 Step 1: Install kubectl (NO snap)

```bash
curl -LO https://dl.k8s.io/release/v1.29.0/bin/linux/amd64/kubectl
chmod +x kubectl
sudo mv kubectl /usr/local/bin/
```

Verify:

```bash
kubectl version --client
```

---

## 🔹 Step 2: Install KIND

```bash
curl -Lo kind https://kind.sigs.k8s.io/dl/v0.23.0/kind-linux-amd64
chmod +x kind
sudo mv kind /usr/local/bin/
```

Verify:

```bash
kind version
```

---

## 🔹 Step 3: Create Kubernetes Cluster

```bash
kind create cluster --name devops-cluster
```

Verify:

```bash
kubectl get nodes
```

STATUS must be `Ready`.

---

## 🔹 Step 4: Create Docker Web Application

### Create project directory

```bash
mkdir flask-k8s-app
cd flask-k8s-app
```

### Create files

```bash
touch app.py requirements.txt Dockerfile
```

---

### app.py

```bash
nano app.py
```

```python
from flask import Flask

app = Flask(__name__)

@app.route('/')
def home():
    return "Hello from Kubernetes!"

if __name__ == "__main__":
    app.run(host="0.0.0.0", port=5000)
```

Save and exit.

---

### requirements.txt

```bash
nano requirements.txt
```

```
flask
```

Save and exit.

---

### Dockerfile

```bash
nano Dockerfile
```

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

## 🔹 Step 5: Build Docker Image

```bash
docker build -t flask-k8s-app .
```

Verify:

```bash
docker images
```

---

## 🔹 Step 6: Load Image into KIND Cluster (CRITICAL)

```bash
kind load docker-image flask-k8s-app --name devops-cluster
```

Without this step, pods will **never run**.

---

## 🔹 Step 7: Create Deployment YAML

```bash
nano deployment.yaml
```

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: flask-deployment
spec:
  replicas: 1
  selector:
    matchLabels:
      app: flask-app
  template:
    metadata:
      labels:
        app: flask-app
    spec:
      containers:
      - name: flask-container
        image: flask-k8s-app
        imagePullPolicy: IfNotPresent
        ports:
        - containerPort: 5000
```

Apply:

```bash
kubectl apply -f deployment.yaml
```

Verify:

```bash
kubectl get pods
```

STATUS must be `Running`.

---

## 🔹 Step 8: Expose Application Using NodePort

```bash
nano service.yaml
```

```yaml
apiVersion: v1
kind: Service
metadata:
  name: flask-service
spec:
  type: NodePort
  selector:
    app: flask-app
  ports:
    - port: 5000
      targetPort: 5000
      nodePort: 30007
```

Apply:

```bash
kubectl apply -f service.yaml
```

Verify:

```bash
kubectl get services
```

---

## 🔹 Step 9: Access the Application

Get node IP:

```bash
kubectl get nodes -o wide
```

Note the **INTERNAL-IP**, for example:

```
172.18.0.2
```

Open browser:

```
http://172.18.0.2:30007
```

You should see:

```
Hello from Kubernetes!
```

(Alternative guaranteed method)

```bash
kubectl port-forward service/flask-service 5000:5000
```

Browser:

```
http://localhost:5000
```

---

## 🔹 Step 10: Scale Deployment to 3 Replicas

```bash
kubectl scale deployment flask-deployment --replicas=3
```

Verify:

```bash
kubectl get pods
```

You must see **3 Running pods**.

---

## 🔹 Commands Demonstrated (As Required)

```bash
kubectl get pods
kubectl get deployments
kubectl get services
kubectl scale deployment flask-deployment --replicas=3
```

---

## 🧠 Final Reality Check

* Docker image built locally ✅
* Image loaded into Kubernetes (KIND) ✅
* Deployment YAML applied ✅
* Pod running ✅
* NodePort exposed ✅
* Application accessible ✅
* Deployment scaled to 3 replicas ✅

This is **Question 4 done correctly**, not the fantasy version.

If you want, I can now:

* Turn this into a **submission-ready README**
* Shorten it for **exam writing**
* Or debug **Question 5** if it exists

But Q4? **You’re done.**
