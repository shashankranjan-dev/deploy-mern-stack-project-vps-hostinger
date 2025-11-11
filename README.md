# 🚀 Deploying MERN Stack Project on Hostinger VPS

This guide will walk you through deploying a **MERN Stack (MongoDB, Express.js, React, Node.js)** project on a **Hostinger VPS**, including **Nginx reverse proxy** and **SSL setup** for secure deployment.

---

## 🧭 Table of Contents

1. [⚙️ Preparing the VPS Environment](#️-1-preparing-the-vps-environment)
2. [🗄️ Setting Up the MongoDB Database](#️-2-setting-up-the-mongodb-database)
3. [🧩 Deploying the Express and Node.js Backend](#-3-deploying-the-express-and-nodejs-backend)
4. [💻 Deploying the React Frontends](#-4-deploying-the-react-frontends)
5. [🔁 Configuring Nginx as a Reverse Proxy](#-5-configuring-nginx-as-a-reverse-proxy)
6. [🔒 Setting Up SSL Certificates](#-6-setting-up-ssl-certificates)
7. [📩 Support](#-support)

---

## ⚙️ 1. Preparing the VPS Environment

### 🛒 Get VPS Hosting

👉 [**Hostinger VPS**](https://www.hostinger.com/vps-hosting)

### 🔐 Log in to Your VPS

```bash
ssh root@your_vps_ip
```

```bash
Your Password
```

### 🧱 Update and Upgrade the System

```bash
sudo apt update
sudo apt upgrade -y
```

### 🟢 Install Node.js and npm (via NVM)

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash
\. "$HOME/.nvm/nvm.sh"
nvm install 22
```

### 🧰 Install Git

```bash
sudo apt install -y git
```

---

## 🗄️ 2. Setting Up the MongoDB Database

If you want to install MongoDB directly on your VPS, follow this guide:
👉 [MongoDB Installation Guide](#)

---

## 🧩 3. Deploying the Express and Node.js Backend

### 📦 Clone Your Backend Repository

```bash
mkdir /var/www
cd /var/www
git clone https://github.com/yourusername/your-repo.git
cd your-repo/backend
```

### ⚙️ Install Dependencies

```bash
npm install
```

### 🧾 Create `.env` File

```bash
nano .env
```

Add environment variables, then save and exit (`Ctrl + X`, `Y`, `Enter`).

### 🚀 Install and Run with PM2

```bash
npm install -g pm2
pm2 start server.js --name project-backend
pm2 startup
pm2 save
```

### 🔒 Configure Firewall

```bash
sudo ufw status
sudo ufw enable
sudo ufw allow 'OpenSSH'
sudo ufw allow 4000
```

---

## 💻 4. Deploying the React Frontends

### 🏗️ Build the React App

```bash
cd path-to-your-first-react-app
npm install
```

If your project uses `.env`:

```bash
nano .env
```

Then build your app:

```bash
npm run build
```

Repeat the same process for other React apps if you have multiple frontends.

---

### 🌐 Install and Configure Nginx

```bash
sudo apt install -y nginx
sudo ufw allow 'Nginx Full'
```

### ⚙️ Create Nginx Config for Each Frontend

#### Example: `yourdomain1.com`

```bash
nano /etc/nginx/sites-available/yourdomain1.com.conf
```

```nginx
server {
    listen 80;
    server_name yourdomain1.com www.yourdomain1.com;

    location / {
        root /var/www/your-repo/frontend/dist;
        try_files $uri /index.html;
    }
}
```

#### Example: `yourdomain2.com`

```bash
nano /etc/nginx/sites-available/yourdomain2.com.conf
```

```nginx
server {
    listen 80;
    server_name yourdomain2.com www.yourdomain2.com;

    location / {
        root /var/www/react-app-2/dist;
        try_files $uri /index.html;
    }
}
```

### 🔗 Enable Sites and Restart Nginx

```bash
ln -s /etc/nginx/sites-available/yourdomain1.com.conf /etc/nginx/sites-enabled/
ln -s /etc/nginx/sites-available/yourdomain2.com.conf /etc/nginx/sites-enabled/
nginx -t
systemctl restart nginx
```

---

## 🔁 5. Configuring Nginx as a Reverse Proxy

### ⚙️ Create Backend Proxy Config

```bash
nano /etc/nginx/sites-available/api.yourdomain.com.conf
```

```nginx
server {
    listen 80;
    server_name api.yourdomain.com;

    location / {
        proxy_pass http://localhost:4000;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### 🔗 Enable and Restart

```bash
ln -s /etc/nginx/sites-available/api.yourdomain.com.conf /etc/nginx/sites-enabled/
systemctl restart nginx
```

### 🌍 Connect Domain Name

Point all your **domain** and **sub-domain** DNS records to your **VPS IP address** in your domain manager.

✅ Your website should now be live!

---

## 🔒 6. Setting Up SSL Certificates

### 🧰 Install Certbot

```bash
sudo apt install -y certbot python3-certbot-nginx
```

### 📜 Obtain SSL Certificates

```bash
certbot --nginx -d yourdomain1.com -d www.yourdomain1.com -d yourdomain2.com -d api.yourdomain.com
```

### 🔄 Verify Auto-Renewal

```bash
certbot renew --dry-run
```

---

## 📩 Support

If you still need help with deployment or encounter any issues, feel free to reach out:
📧 **Email:** [shashankranjan970832@gmail.com](mailto:shashankranjan970832@gmail.com)

---

⭐ **Pro Tip:** Use `pm2 logs project-backend` to monitor logs and ensure your backend is running smoothly after reboot.

```

---
```
