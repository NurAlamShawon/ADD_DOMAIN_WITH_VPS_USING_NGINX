# ADD_DOMAIN_WITH_VPS_USING_NGINX


# Project Title

A brief description of what this project does and who it's for

# VPS Deployment Guide

### Ubuntu • Node.js • PM2 • Nginx • SSL • GitHub SSH

---

## 0️⃣ Connect to VPS

### SSH Login (Password Method)

```bash
ssh root@YOUR_SERVER_IP
Enter your VPS password when prompted.

If using another user:

```
```bash
ssh username@YOUR_SERVER_IP
```
## 1️⃣ System Update

```bash

sudo apt update
sudo apt upgrade -y
sudo apt install git curl build-essential -y

```
## 2️⃣ Set up GitHub Access (SSH Key)


```bash
Generate SSH Key
```
```bash
ssh-keygen -t ed25519 -C "your-email@example.com"
Press Enter for default path

Press Enter for no passphrase

Show Public Key
```
```bash
cat ~/.ssh/id_ed25519.pub
Copy the output and add it to GitHub:

GitHub → Settings → SSH and GPG keys → New SSH Key

Test GitHub Connection
```
```bash
ssh -T git@github.com
```
## 3️⃣ Clone Project
```bash
Use SSH URL, not HTTPS

```
```bash
git clone git@github.com:username/repo.git
cd repo
```
## 4️⃣ Install Node.js (NVM)

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.1/install.sh | bash
source ~/.bashrc
nvm install --lts
node -v
npm -v
```
## 5️⃣ Install Dependencies

```bash
npm install
```
## 6️⃣ Add Environment Variables

```bash
nano .env
Paste your environment variables and save.

```
## 7️⃣ Build Project
```bash
npm run build
```
## 8️⃣ Run App with PM2
```bash
Install PM2
```
```bash
sudo npm install -g pm2
Start App
```
```bash
pm2 start npm --name "app" -- run start
Save & Enable Startup
```
```bash
pm2 save
pm2 startup systemd
View Logs
```
```bash
pm2 logs app
```
## 9️⃣ Install & Configure Nginx

## Install Nginx

```bash
sudo apt install nginx -y
sudo systemctl enable nginx
sudo systemctl start nginx
Remove Default Site
```
```bash
sudo rm /etc/nginx/sites-enabled/default
sudo rm /etc/nginx/sites-available/default
```
## 🔟 Create Nginx Config

```bash
sudo nano /etc/nginx/sites-available/api.example.com
Paste:

server {
    listen 80;
    server_name api.example.com;

    location / {
        proxy_pass http://127.0.0.1:5000;
        proxy_http_version 1.1;

        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}


```
## Enable & reload:
```bash

sudo ln -s /etc/nginx/sites-available/api.example.com /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl restart nginx
```
## 1️⃣1️⃣ Install SSL (Certbot)

```bash
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d api.example.com
sudo certbot renew --dry-run
```
## 1️⃣2️⃣ Firewall Setup (UFW)
```bash
sudo ufw allow OpenSSH
sudo ufw allow 'Nginx Full'
sudo ufw enable
sudo ufw status
```
## 1️⃣3️⃣ Deployment & Maintenance (Optional)
```bash
Pull Latest Code
```
```bash
git pull origin main
Install New Dependencies

```
```bash
npm install
Prisma (If Used)
```
```bash
npx prisma generate
npx prisma migrate deploy
Build
```
```bash
npm run build
Restart App
```
```bash
pm2 restart app
Useful PM2 Commands
```
```bash
pm2 ls
pm2 logs app
pm2 show app
pm2 restart app
pm2 stop app
pm2 save
```

## 1️⃣4️⃣ Quick Checklist Before Redeploying
```
 .env updated

 Dependencies installed

 Database migrations applied

 Build successful

 PM2 restarted
