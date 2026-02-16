Jenkins Credentials:

```
Username: i1s_akashatdev/i1s_akashatjenkins
Password: 81ba8a4c9e29475498560b64f5afbe0a/Jenkins@123/0b7a847bfec24f28b00f38df564a44fe
```

Jenkins URL: `http://localhost:8080/` / `http://23.20.83.223:8080/` / `http://i1sakash.jumpingcrab.com/` / `http://i1sakashatfrontend.jumpingcrab.com/`

<!-- Check webhook -->

AWS Credentials:

```
My Mail
Root User
Password: Aws@09****
```

# 🚀 React CI Pipeline using Jenkins

# 🔁 CI Flow

```
Git Push
   ↓
Jenkins Job Triggered (Webhook)
   ↓
Install Dependencies
   ↓
Lint + Test (Optional)
   ↓
Build React App
   ↓
Deploy build/ to Server
```

---

# 📚 Table of Contents

1. Jenkins Setup (macOS)
2. NodeJS Plugin Configuration
3. Pipeline Setup
4. GitHub Webhook Setup
5. Verification
6. Final Result
7. ngrok?
8. Next Steps

---

# ⚙️ 1️⃣ Jenkins Setup (macOS)

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

---

# 🟢 2️⃣ NodeJS Plugin Configuration

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

# 🏗 3️⃣ Pipeline Setup

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

        stage('Verify Node') {
            steps {
                sh 'node -v'
                sh 'npm -v'
            }
        }

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/i1s-akash/react-jenkins.git'
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
    }

    post {
        success {
            echo '✅ React build successful'
        }
        failure {
            echo '❌ React build failed'
        }
    }
}
```

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

Content type:
application/json

Event:
Just the push event
```

Save.

Now every `git push` automatically triggers Jenkins.

---

# 🧪 5️⃣ Verification

Check Node & npm:

```bash
node -v
npm -v
```

Push a commit:

```bash
git commit -am "test webhook"
git push
```

Jenkins should auto-trigger and build successfully.

---

# 🏁 6. Final Result

You now have:

✔ Jenkins running on macOS  
✔ NodeJS 20 integrated  
✔ Declarative CI pipeline  
✔ GitHub webhook automation  
✔ React production build

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
<!--  -->
