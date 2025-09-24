# Expense Tracker - Frontend

This is the React-based frontend for the Expense Tracker Portal. It provides a user-friendly interface for users to manage their expenses.

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

Install Nginx
```
sudo dnf install nginx -y 
```
Enable nginx
```
sudo systemctl enable nginx
```
Start nginx
```
sudo systemctl start nginx
```
Try to access the service once over the browser and ensure you get some default content

Remove the default content that web server is serving.
```
rm -rf /usr/share/nginx/html/*
```

---
## Folder Structure

- `src/components/` - React components (HomePage, AddStudentForm, EditStudentForm, etc.)
- `src/services/` - API service for backend communication
- `public/` - Static assets

---
