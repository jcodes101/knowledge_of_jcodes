=========== DEPLOYING EC2 INSTANCE ON AWS FOR FRONT-END USING UBUNTU ===========

1️⃣ Create EC2 Ubuntu Server + name it + choose ubuntu image + use t<<num>> micro instance type + create key pair

    Network settings:
    	+ Allow SSH traffic from
    	+ Allow HTTPS traffic from the internet
    	+ Allow HTTP traffic from the internet
    		(For hackathon, ports can be 0.0.0.0/0)

    Launch Instance

===============================================================================

2️⃣ Install System & Tools on EC2

# Update Ubuntu

sudo apt update && sudo apt upgrade -y

# Install Git

sudo apt install git -y

===============================================================================

3️⃣ Install Node.js & Build Tools

This script:

- Adds the NodeSource APT repository to your system
- Imports the GPG signing key
- Updates your apt package list
- 4: Prepares apt to install Node.js LTS (like sudo apt install nodejs afterward)
- It does not install Node.js directly — it only sets up the repository.

curl -fsSL https://deb.nodesource.com/setup_lts.x | sudo -E bash -

sudo apt install -y nodejs build-essential

# Check versions

node -v
npm -v

(Recommended but optional)
sudo npm install -g pm2

===============================================================================

4️⃣ Install & Configure Nginx

sudo apt install nginx -y
sudo systemctl enable --now nginx

Edit React config:

sudo nano /etc/nginx/sites-available/default

Replace location / { ... } block:

location / {
try_files $uri $uri/ /index.html;
root /var/www/html;
index index.html;
}

or

server {
listen 80 default_server;
listen [::]:80 default_server;

    server_name _;

    location / {
        proxy_pass http://127.0.0.1:5173;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }

}

    Action Keys To EXIT:
    	= Save (write file)	Ctrl + O
    	= Confirm filename	Enter
    	= Exit editor	Ctrl + X

Restart Nginx:

sudo nginx -t
sudo systemctl restart nginx
sudo systemctl status nginx

===============================================================================

5️⃣ Create React App on EC2

mkdir ~/projects && cd ~/projects

# Create Vite React app

npm create vite@latest be-smart-frontend -- --template react

cd be-smart-frontend
npm install

npm run dev -- --host 0.0.0.0

===============================================================================

6️⃣ Initialize Git & Push to GitHub

git init
git config --global user.name "Your Name"
git config --global user.email "you@email.com"

git add .
git commit -m "Initial app setup"
git branch -M main

QUICK DETOUR:->
Go to GitHub
→ click New repository

    	Name it be-smart-frontend (in this case)

    	DO NOT initialize with README, .gitignore, or license

# Add remote repo (replace with your URL)

git remote add origin https://github.com/USERNAME/be-smart-frontend.git
git push -u origin main

    ++ IF Sign-In DOES NOT WORK:
    	Option 1 — Use a Personal Access Token (PAT) with HTTPS

    	Go to GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic) → 		Generate new token

    	Select scopes: repo (for full repo access)

    	Copy the token (this is your “password” for Git)

    	Push using HTTPS with the token:
    		git push https://<TOKEN>@github.com/jcodes101/be-smart-frontend.git main

    	ex.
    		git push https://<<TOKEN>>@github.com/jcodes101/be-smart-frontend.git main

GET YOUR KEY AND REPLACE IT AND PUT IT IN THE ACTUAL CMD

ex. git push https://github_pat<<TOKEN>>@github.com/jcodes101/be-smart-frontend.git main

😭 LAST Step: Clone into Cursor (your laptop)

cd ~/Documents # or wherever you keep projects
git clone https://github.com/jcodes101/be-smart-frontend.git
cd be-smart-frontend
npm install
npm run dev

IF YOU EVER LEAKED SOMETHING IN A COMMIT:
git reset --soft HEAD~1

    ^This undoes the last commit but keeps your file changes.
