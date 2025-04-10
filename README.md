# Jenkins CI/CD Pipeline with Dockerized Node.js App

This project demonstrates how to build a basic CI/CD pipeline using **Jenkins** to automate the build and deployment of a **Node.js** application inside a **Docker** container.

---

## 🔧 Prerequisites

- Docker installed and running
- Jenkins installed (running on port `9090`)
- GitHub account and repo (e.g., `https://github.com/Yash5103/jenkins-pipeline-demo`)
- Basic Node.js project files (`index.js`, `package.json`, `Dockerfile`, `Jenkinsfile`)

---

## 📁 Project Structure

jenkins-pipeline-demo/ │ ├── index.js ├── package.json ├── Dockerfile └── Jenkinsfile

yaml
Copy
Edit

---

## 📦 Step-by-Step Setup

### 1. **Create Node.js App**
Create `index.js`:
```js
const express = require('express');
const app = express();

app.get('/', (req, res) => res.send('Hello from Node App deployed via Jenkins!'));
app.listen(3000, () => console.log('App listening on port 3000'));
Create package.json:

json
Copy
Edit
{
  "name": "nodejs-jenkins-app",
  "version": "1.0.0",
  "main": "index.js",
  "scripts": {
    "start": "node index.js"
  },
  "dependencies": {
    "express": "^4.18.2"
  }
}
Install dependencies:

bash
Copy
Edit
npm install
2. Create Dockerfile
Dockerfile
Copy
Edit
FROM node:18

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3000
CMD ["npm", "start"]
3. Create Jenkinsfile
groovy
Copy
Edit
pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building the Docker image...'
                sh 'docker build -t node-app .'
            }
        }

        stage('Test') {
            steps {
                echo 'Running tests...'
                sh 'echo No tests configured.'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying the app...'
                sh 'docker rm -f node-app-container || true'
                sh 'docker run -d -p 3000:3000 --name node-app-container node-app'
            }
        }
    }
}
4. Push Project to GitHub
bash
Copy
Edit
git init
git remote add origin https://github.com/Yash5103/jenkins-pipeline-demo.git
git add .
git commit -m "Initial commit with Node.js app and Jenkinsfile"
git push -u origin main
5. Configure Jenkins Pipeline
Open Jenkins (http://localhost:9090)

Click "New Item" → Enter name → Select Pipeline

In the config:

Check "Git"

Enter GitHub repo URL: https://github.com/Yash5103/jenkins-pipeline-demo.git

Scroll to Pipeline section → choose "Pipeline script from SCM"

Branch: main

Script path: Jenkinsfile

Click Save → Click Build Now

✅ Verify Deployment
Ensure Docker container is running:

bash
Copy
Edit
docker ps
Test app:

bash
Copy
Edit
curl http://localhost:3000
You should see:

csharp
Copy
Edit
Hello from Node App deployed via Jenkins!
🧹 Common Errors & Fixes
Error	Solution
Cannot find module 'express'	Run npm install and rebuild Docker image
curl: (7) Failed to connect to localhost port 3000	Check if the container is running and port is mapped
✅ Final Output
A successfully running Node.js app on http://localhost:3000, deployed automatically via Jenkins pipeline, built and run inside Docker container.

📌 Summary
This task automated:

Building a Docker image for a Node.js app

Running the app in a container

Triggering these steps via Jenkins pipeline on every commit
