# 🚀 React CI/CD Deployment using Jenkins + Nginx + AWS EC2

This project demonstrates a complete CI/CD pipeline for building and deploying a React (Vite) application to production using:

AWS EC2 (Ubuntu 22.04)
Jenkins (CI/CD Server)
Node.js 20 LTS
Nginx (Reverse Proxy + Static Hosting)
GitHub Webhooks
Ngrok(For local jenkins deployment)

---

# 🌐 Live URLs

- 🔧 Jenkins: http://i1sakash.jumpingcrab.com/
- 🌍 Frontend: http://i1sakashatfrontend.jumpingcrab.com/

```
My Mail
Root User
Password: **********
```

---

# 🏗 Architecture Flow

```
GitHub Push
      ↓
Jenkins (EC2)
      ↓
npm ci
      ↓
npm run build
      ↓
dist/
      ↓
/var/www/frontend
      ↓
Nginx
      ↓
Public Domain
```

---

# 📚 Table of Contents

1. AWS Setup
2. EC2 Setup
3. Networking & Architecture
4. Jenkins Installation (EC2)
5. Jenkins Setup (macOS - Local Dev)
6. NodeJS Plugin Configuration
7. Nginx Setup
8. Pipeline Setup
9. Permissions Setup
10. Build & Deployment Strategy
11. GitHub Webhook Setup
12. Performance Optimization
13. Verification
14. Final Result
15. ngrok (Local Webhook Testing)
16. Next Steps

---

# 1️⃣ AWS Setup

- Create AWS account
- Use IAM user (do NOT use root for daily work)
- Select region
- Generate SSH key pair

---

# 2️⃣ EC2 Setup

- Launch Ubuntu 22.04
- Instance type: t2.micro
- Enable auto-assign public IP
- Configure Security Group:

| Port | Purpose                 |
| ---- | ----------------------- |
| 22   | SSH                     |
| 80   | HTTP                    |
| 443  | HTTPS                   |
| 8080 | Jenkins (Internal only) |

Connect via SSH:

```bash
ssh -i your-key.pem ubuntu@<public-ip>
```

---

# 3️⃣ Networking & Architecture

## Ports Used

- 22 → SSH
- 80 → HTTP
- 443 → HTTPS (future)
- 8080 → Jenkins

# 4️⃣ Jenkins Installation (EC2)

Install Java:

```bash
sudo apt update
sudo apt install openjdk-17-jdk -y
```

Install Jenkins:

```bash
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo tee \
  /usr/share/keyrings/jenkins-keyring.asc > /dev/null

echo deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] \
  https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
  /etc/apt/sources.list.d/jenkins.list > /dev/null

sudo apt update
sudo apt install jenkins -y
```

Start Jenkins:

```bash
sudo systemctl start jenkins
```

Get admin password:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

---

# 5️⃣ Jenkins Setup (macOS - Optional Local Setup)

## Install Homebrew

Check if installed:

```bash
brew --version
```

If not installed:

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

---

## Install Java (Required for Jenkins)

```bash
brew install openjdk@17
```

Link Java:

```bash
sudo ln -sfn $(brew --prefix openjdk@17)/libexec/openjdk.jdk \
  /Library/Java/JavaVirtualMachines/openjdk-17.jdk
```

Verify:

```bash
java -version
```

You should see Java 17.

---

## Install Jenkins (LTS)

```bash
brew install jenkins-lts
```

Start Jenkins:

```bash
brew services start jenkins-lts
```

Stop Jenkins:

```bash
brew services stop jenkins-lts
```

Verify:

```bash
brew services list
```

You should see:

```
jenkins-lts started
```

---

## Access Jenkins

Open in browser:

```
http://localhost:8080
```

---

# 6️⃣ NodeJS Plugin Configuration

## Get Initial Admin Password

```bash
cat ~/.jenkins/secrets/initialAdminPassword
```

Paste it into the browser and continue setup.

---

## Install Recommended Plugins

Choose:

```
Install suggested plugins
```

This installs:

- Git
- Pipeline
- Credentials Binding
- SSH
- UI plugins

Create an admin user and complete setup.

NodeJS Plugin Configuration

## Install NodeJS Plugin

Go to:

```
Manage Jenkins → Plugins → Available
```

Search:

```
NodeJS
```

Install and restart Jenkins if prompted.

---

## Configure NodeJS Tool

Go to:

```
Manage Jenkins → Tools → NodeJS
```

Add:

```
Name: node-20
Version: 20.x (Latest LTS)
Install automatically: ✅
```

Save.

---

# 7️⃣ Nginx Setup

Install:

```bash
sudo apt install nginx -y
```

## Jenkins Reverse Proxy

File:

```
/etc/nginx/sites-available/jenkins
```

```nginx
server {
    listen 80;
    server_name i1sakash.jumpingcrab.com;

    location / {
        proxy_pass http://127.0.0.1:8080;
        proxy_set_header Host $host;
    }
}
```

---

## Frontend Static Hosting

File:

```
/etc/nginx/sites-available/frontend
```

```nginx
server {
    listen 80;
    server_name i1sakashatfrontend.jumpingcrab.com;

    root /var/www/frontend;
    index index.html;

    location / {
        try_files $uri /index.html;
    }
}
```

Enable sites:

```bash
sudo ln -s /etc/nginx/sites-available/frontend /etc/nginx/sites-enabled/
sudo systemctl restart nginx
```

---

# 8️⃣ Pipeline Setup (Jenkinsfile)

Create a new **Pipeline Job** in Jenkins.

Add this `Jenkinsfile` in your project root:

```groovy
pipeline {
    agent any

    tools {
        nodejs 'node-20'
    }

    environment {
        CI = 'false'
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm ci'
            }
        }

        stage('Build') {
            steps {
                sh 'npm run build'
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    rm -rf /var/www/frontend/*
                    cp -r dist/. /var/www/frontend/
                '''
            }
        }
    }
}
```

---

# 9️⃣ Permissions Setup

Allow Jenkins to deploy:

```bash
sudo chown -R jenkins:jenkins /var/www/frontend
sudo chmod -R 755 /var/www/frontend
```

---

# 🔟 Build & Deployment Strategy

- Use `npm ci` for clean installs
- Build inside Jenkins
- Deploy static files to Nginx
- No sudo inside Jenkinsfile
- Reverse proxy isolates Jenkins from public exposure

---

# 🔔 4️⃣ GitHub Webhook Setup (Local Development)

## Expose Jenkins (Temporary Local Access)

```bash
ngrok http 8080
```

Copy HTTPS forwarding URL:

```
https://your-ngrok-url.ngrok-free.dev
```

---

## Add Webhook in GitHub

Go to:

```
Repository → Settings → Webhooks → Add webhook
```

Set:

```
Payload URL:
https://your-ngrok-url.ngrok-free.dev/github-webhook/
http://i1sakash.jumpingcrab.com/github-webhook/

Content type:
application/json

Event:
Just the push event
```

Save.

Now every `git push` automatically triggers Jenkins.

---

# 1️⃣2️⃣ Performance Optimization

Enable gzip in `/etc/nginx/nginx.conf`:

```nginx
gzip on;
gzip_types text/plain text/css application/javascript;
```

Enable static caching:

```nginx
location ~* \.(js|css|png|jpg|jpeg|gif|ico|svg)$ {
    expires 7d;
}
```

---

# 1️⃣3️⃣ Verification

Push a commit:

```bash
git commit -am "Test"
git push
```

Pipeline should:

- Install dependencies
- Build React app
- Deploy automatically

---

# 7. What is ngrok?

ngrok is a tunneling tool that exposes your local development server to the internet using a secure public URL.

It allows external services (like payment gateways, OAuth providers, or webhooks) to access your locally running application.

---

## Why Use ngrok?

- Test webhooks (Stripe, Razorpay, GitHub, etc.)
- Test OAuth callback URLs (Google login, AWS Cognito, etc.)
- Share local development builds with teammates or clients
- Test mobile apps connected to a local backend

---

## How It Works

If your application runs locally at:

http://localhost:3000

Run the following command:

```bash
ngrok http 3000
```

ngrok will generate a public URL like:

https://abc123.ngrok.io

All traffic to this public URL will be securely forwarded to your local server.

---

## Installation

1. Download ngrok from: https://ngrok.com
2. Install and authenticate:

```bash
ngrok config add-authtoken <your-auth-token>
```

3. Start tunnel:

```bash
ngrok http <port>
```

---

## Notes

- Free plan URLs change every time you restart ngrok.
- Paid plans allow custom domains and reserved URLs.

---

# 🔜 Next Steps

- Deploy Jenkins on AWS EC2 (24/7 availability)
- Add Role-Based Access Control (RBAC)
- Add Deployment stage (Nginx / S3)
- Add Docker-based pipeline
- Secure Jenkins with HTTPS

<!-- Root User vs IAM User -->
<!-- jetty web server -->
<!-- Need a web server to deploy the build/folder -->
<!-- What is web server ? -->
<!-- EC2 Setup, Billing Budget vs Cloudwatch -->
<!-- Jenkins -->
<!-- Nginx -->
<!-- GitHub Webhook -->
<!-- Linus Fundamentals -->
