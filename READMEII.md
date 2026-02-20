# 🚀 React CI/CD Deployment using GitHub Actions + Nginx + AWS EC2

This project demonstrates a complete CI/CD pipeline for building and deploying a React (Vite) application to production using:

- AWS EC2 (Ubuntu 22.04)
- GitHub Actions (Cloud CI/CD Runner)
- Node.js 20 LTS
- Nginx (Static Hosting + Reverse Proxy)
- GitHub Secrets (Secure SSH Deployment)

---

# 🌐 Live URL

🌍 Frontend: `http://YOUR_PUBLIC_IP`

---

# 🏗 Architecture Flow

```
GitHub Push
      ↓
GitHub Actions (Cloud Runner)
      ↓
npm ci
      ↓
npm run build
      ↓
dist/
      ↓
SCP to EC2 (/var/www/app)
      ↓
Restart Nginx
      ↓
Public IP
```

---

# 📚 Table of Contents

1. AWS Setup
2. EC2 Setup
3. Security Group Configuration
4. Nginx Installation
5. GitHub Secrets Setup
6. GitHub Actions Workflow
7. Deployment Strategy
8. Permissions Setup
9. Verification
10. Common Errors & Fixes
11. Next Steps

---

# 1️⃣ AWS Setup

- Create AWS account
- Create IAM user (avoid using root daily)
- Select region
- Generate SSH key pair (`.pem` file)

---

# 2️⃣ EC2 Setup

- Launch Ubuntu 22.04 instance
- Instance type: `t2.micro`
- Enable auto-assign public IP

Connect via SSH:

```bash
ssh -i your-key.pem ubuntu@<public-ip>
```

---

# 3️⃣ Security Group Configuration

Configure inbound rules:

| Port | Purpose          |
| ---- | ---------------- |
| 22   | SSH              |
| 80   | HTTP             |
| 443  | HTTPS (Optional) |

---

# 4️⃣ Nginx Installation & Setup

## Install Nginx

```bash
sudo apt update
sudo apt install nginx -y
```

Enable & start:

```bash
sudo systemctl enable nginx
sudo systemctl start nginx
```

---

## Create Deployment Directory

```bash
sudo mkdir -p /var/www/app
sudo chown -R ubuntu:ubuntu /var/www/app
sudo chmod -R 755 /var/www/app
```

---

## Configure Nginx

Edit:

```bash
sudo nano /etc/nginx/sites-available/default
```

Replace with:

```nginx
server {
    listen 80;
    server_name _;

    root /var/www/app;
    index index.html;

    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

Test configuration:

```bash
sudo nginx -t
sudo systemctl restart nginx
```

---

# 5️⃣ GitHub Secrets Setup

Go to:

```
Repository → Settings → Secrets and Variables → Actions
```

Add the following secrets:

| Secret Name | Value                                |
| ----------- | ------------------------------------ |
| EC2_HOST    | EC2 Public IPv4 address              |
| EC2_SSH_KEY | Full private key (.pem file content) |

Private key format:

```
-----BEGIN RSA PRIVATE KEY-----
...
-----END RSA PRIVATE KEY-----
```

---

# 6️⃣ GitHub Actions Workflow

Create file:

```
.github/workflows/deploy.yml
```

Add:

```yaml
name: 🚀 React Production CI/CD

on:
  push:
    branches:
      - main

jobs:
  build-and-deploy:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: 20
          cache: npm

      - name: Install Dependencies
        run: npm ci

      - name: Build Project
        run: npm run build

      - name: Deploy to EC2 via SCP
        uses: appleboy/scp-action@v0.1.7
        with:
          host: ${{ secrets.EC2_HOST }}
          username: ubuntu
          key: ${{ secrets.EC2_SSH_KEY }}
          source: "dist/*"
          target: "/var/www/app"

      - name: Restart Nginx
        uses: appleboy/ssh-action@v1.0.3
        with:
          host: ${{ secrets.EC2_HOST }}
          username: ubuntu
          key: ${{ secrets.EC2_SSH_KEY }}
          script: |
            sudo systemctl restart nginx
```

---

# 7️⃣ Deployment Strategy

- `npm ci` ensures clean dependency installation
- Build happens inside GitHub cloud runner
- Only optimized production `dist/` folder is deployed
- SCP securely copies files to EC2
- Nginx serves static files
- No Node server required in production

---

# 8️⃣ Permissions Setup (If Required)

If deployment fails due to permissions:

```bash
sudo chown -R ubuntu:ubuntu /var/www/app
sudo chmod -R 755 /var/www/app
```

---

# 9️⃣ Verification

Push a commit:

```bash
git commit -am "Test deployment"
git push
```

Check:

- GitHub Actions → Workflow status
- Open browser → `http://YOUR_PUBLIC_IP`

---

# 🔟 Common Errors & Fixes

### 🔴 ssh: no key found

→ Invalid or incomplete SSH private key

### 🔴 i/o timeout

→ Port 22 not open  
→ Wrong EC2 IP

### 🔴 500 Internal Server Error

→ Incorrect Nginx root path

### 🔴 Redirect Loop

Ensure:

```nginx
try_files $uri $uri/ /index.html;
```

---

# 🔜 Next Steps

- Add SSL using Let's Encrypt
- Add staging & production environments
- Dockerize application
- Add automated testing
- Implement Blue-Green deployment
- Setup monitoring & logging

---

# 🏁 Final Result

✔ Automated CI/CD pipeline  
✔ Secure SSH-based deployment  
✔ Production-ready Nginx configuration  
✔ Live React application hosted on AWS

---

# 👨‍💻 Author

**Akash Dev**  
DevOps / Frontend Engineer
