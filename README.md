
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
```

Verify:

```bash
java -version
```

---

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

---

## 🔹 Commands Demonstrated (As Required)

```bash
git init
git add
git commit
git push
```

Jenkins:

* Freestyle Job
* Git SCM
* Webhook-triggered build

