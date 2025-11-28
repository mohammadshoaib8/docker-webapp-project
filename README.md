# 🛠️ Tools & Technologies Used

| Tool | Logo |
|------|------|
| **Docker** | ![Docker](https://raw.githubusercontent.com/devicons/devicon/master/icons/docker/docker-original.svg?v=4&s=50) |
| **Jenkins** | ![Jenkins](https://raw.githubusercontent.com/devicons/devicon/master/icons/jenkins/jenkins-original.svg?v=4&s=50) |
| **GitHub** | ![GitHub](https://raw.githubusercontent.com/devicons/devicon/master/icons/github/github-original.svg?v=4&s=50) |
| **NGINX** | ![Nginx](https://raw.githubusercontent.com/devicons/devicon/master/icons/nginx/nginx-original.svg?v=4&s=50) |
| **Linux** | ![Linux](https://raw.githubusercontent.com/devicons/devicon/master/icons/linux/linux-original.svg?v=4&s=50) |

---
Docker NGINX WebApp Project  
A simple yet professional **Dockerized NGINX web application** deployed using a **Jenkins CI/CD pipeline**.  
This project demonstrates how to build, push, and run Docker containers automatically using Jenkins.

---

## 🚀 Project Overview

This project includes:
- A **custom Dockerfile**
- A simple **HTML webpage with images**
- **Jenkins Declarative Pipeline** for CI/CD
- Automatic Docker **build → push → deploy**
- Hosting on **Docker Hub**
- Running container on port **8000**

---

## 📁 Project Structure

```

.
├── Dockerfile
├── index.html
└── images/
├── devops.png
├── cloud.png
└── team.png

````

# 📸 Webpage Preview

![Web Screenshot](https://raw.githubusercontent.com/mohammadshoaib8/docker-webapp-project/main/images/preview.png)

---

# 📦 Dockerfile

```dockerfile
FROM nginx:latest

COPY index.html /usr/share/nginx/html/index.html
COPY images/ /usr/share/nginx/html/images/

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]
````

---

# 🌈 How to Run Locally

### ▶️ **1. Clone the Repository**

```bash
git clone https://github.com/mohammadshoaib8/docker-webapp-project.git
cd docker-webapp-project
```

### ▶️ **2. Build the Docker Image**

```bash
docker build -t webapp:latest .
```

### ▶️ **3. Run the Container**

```bash
docker run -d -p 8000:80 --name mywebapp webapp:latest
```

### ▶️ **4. Open in Browser**

```
http://localhost:8000
```

---

# 🔥 Jenkins CI/CD Pipeline (Declarative)

```groovy
pipeline {
    agent any

    environment {
        IMAGE_NAME = "msshoaib2255457/testdocker"
        IMAGE_TAG  = "v${BUILD_NUMBER}"
    }

    stages {
        stage("Git clone") {
            steps {
                git credentialsId: '5344fe9e-333a-4e01-844b-fc56f14330fd', 
                    url: 'https://github.com/mohammadshoaib8/docker-webapp-project.git'
            }
        }

        stage("Docker build") {
            steps {
                sh '''
                    docker build -t $IMAGE_NAME:$IMAGE_TAG .
                    docker tag $IMAGE_NAME:$IMAGE_TAG $IMAGE_NAME:latest
                '''
            }
        }

        stage("Docker push") {
            steps {
                withDockerRegistry(credentialsId: 'dockerhub', url: 'https://index.docker.io/v1/') {
                    sh '''
                        docker push $IMAGE_NAME:$IMAGE_TAG
                        docker push $IMAGE_NAME:latest
                    '''
                }
            }
        }

        stage ("Docker Deploy") {
            steps {
                sh '''
                    docker rm -f nginxapp || true
                    docker run -d --name nginxapp -p 8000:80 $IMAGE_NAME:latest
                '''
            }
        }
    }
}
```

---

# 🧪 How the Pipeline Works

| Stage             | Description                                    |
| ----------------- | ---------------------------------------------- |
| **Git clone**     | Pulls latest code from GitHub                  |
| **Docker build**  | Builds Docker image for the HTML+NGINX web app |
| **Docker push**   | Pushes image to Docker Hub                     |
| **Docker deploy** | Runs a fresh container on port 8000            |

---

# 🌍 Access the App After Deployment

```
http://<your-server-ip>:8000
```

---

# 🐳 Docker Hub Repository

🔗 [https://hub.docker.com/r/msshoaib2255457/testdocker](https://hub.docker.com/r/msshoaib2255457/testdocker)

---

# 📧 Contact

**Author:** Shaik Mohammad Shoaib
**GitHub:** [https://github.com/mohammadshoaib8](https://github.com/mohammadshoaib8)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-blue?logo=linkedin)](https://www.linkedin.com/in/mohammadshoaib8)
