# Expense Tracker - Frontend

This is the React-based frontend for the Expense Tracker Portal. It provides a user-friendly interface for users to manage their expenses and income records.

---

##  Technologies Used

- **React** (via Create React App)
- **JavaScript**
- **Axios** – HTTP client for API calls
- **Bootstrap / Material-UI** – UI Styling (optional)
- **Node.js + npm** – Runtime and package manager

---

## Step-by-Step Installation & Deployment


### 1. Prerequisites

#### Install Node.js and npm (Step-by-Step)

Node.js is required to run the frontend. npm (Node Package Manager) comes bundled with Node.js.

**Step 1: Check if Node.js and npm are already installed**
```
node -v
npm -v
```
If you see version numbers, Node.js and npm are already installed. If not, follow the steps below.

**Step 2: Download Node.js**

- Go to the [Node.js official website](https://nodejs.org/).
- Download the LTS (Long Term Support) version for your operating system (Windows, macOS, or Linux).

**Step 3: Install Node.js**

- On Linux, you can also use a package manager. On RHEL:
```
curl -fsSL https://rpm.nodesource.com/setup_18.x | sudo bash -
sudo yum install -y nodejs
```
Or for Ubuntu:
```
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs
```

**Step 4: Verify Installation**
```
node -v
npm -v
```
You should see version numbers for both Node.js and npm.

---

---

### 2. Clone the Frontend Repository

Navigate to your desired directory and clone the repository:
```
cd /opt
sudo git clone https://github.com/Nareshindu/expense-frontend.git
sudo chown -R ec2-user:ec2-user /opt/expense-frontend
cd /opt/Student-portal-frontend/expense-frontend

```

---

### 3. Install Dependencies(When you are running npm install command you should run this command where package.json is there )

Install all required npm packages:
```
cd expense-frontend
```
```
npm install
```

---


### 4. Build for Production

To create an optimized production build:
```
npm run build
```
The build output will be in the `build/` directory.

---

The frontend is the service in Expense to serve the web content over Nginx. This will have the web frame for the web application.

This is a static content and to serve static content we need a web server. This server

Developer has chosen Nginx as a web server and thus we will install Nginx Web Server.

## Install Nginx Web Server
Install Nginx using dnf (applicable to RHEL, CentOS, or Fedora-based systems):
```
sudo dnf install nginx -y 
```
Enable Nginx to start on boot:
```
sudo systemctl enable nginx
```
Start the Nginx service:
```
sudo systemctl start nginx
```
## Verify Default Nginx Web Page
Open a browser and access your server’s IP (e.g., http://your-server-ip). You should see the default Nginx welcome page.

Remove existing default content:
```
rm -rf /usr/share/nginx/html/*
```
## Deploy the Frontend Build

Copy the built frontend files to the Nginx HTML directory:
```
sudo cp -r /opt/expense-frontend/build/* /usr/share/nginx/html/
```

## Configure Nginx as a Reverse Proxy

Open the Nginx configuration file (or create one if it doesn’t exist):

```
sudo vi /etc/nginx/conf.d/default.conf
```
Paste the following configuration:
```
server {
    listen 80;
    server_name _;

    root /usr/share/nginx/html;
    index index.html;

    location / {
        try_files $uri /index.html;
    }

    # Proxy API calls to backend private IP
    location /api/ {
        proxy_pass http://backendserver-ipaddress:8080/api/;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
        proxy_set_header X-Real-IP $remote_addr;
    }

    error_page 404 /index.html;
}
```
Note:- Replace <backend-server-ip> with the private IP address or hostname of your backend server.


## Validate Nginx Configuration Before Restart
Before restarting, validate the syntax:
```
sudo nginx -t
```

## Restart Nginx to Apply Changes
```
sudo systemctl restart nginx
```
---
## Folder Structure

- `src/components/` - React components (EntryForm, EntryList, Report, etc.)
- `src/services/` - API service for backend communication
- `public/` - Static assets

---
