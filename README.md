# Hello Eyego - Node.js App Deployment (Manual & Automated via Jenkins)

## Manual Deployment Steps

### 1. Launch EC2 and Install Docker

* Launched EC2 instance (Amazon Linux 2)
* Installed Docker:

  ```bash
  sudo yum update -y 
  sudo yum install docker -y
  sudo service docker start
  sudo usermod -aG docker ec2-user
  ```

### 2. Clone GitHub Repository

* Cloned repository with files:

  * `index.js`
  * `package.json`
  * `Dockerfile`

  ```bash
  git clone https://github.com/Ritesh-Bhapkar/Eyego-Ai-Task.git
  cd Eyego-Ai-Task
  ```

### 3. Build Docker Image

```bash
docker build -t nodejs-app .
```

### 4. Run Docker Container

```bash
docker run -d -p 8080:3000 nodejs-app
```

* Allowed port `8080` in EC2 security group
* Accessed app at `http://<EC2-IP>:8080`

---

## Push Image to Docker Hub

```bash
docker login
docker tag nodejs-app ritesh605/nodejs-app
docker push ritesh605/nodejs-app
```

---

## Kubernetes Deployment on EKS

### 1. Created EKS Cluster

```bash
eksctl create cluster --name ritesh-cluster4 --region ap-southeast-2 --node-type t2.medium ap-southeast-2a
```

### 2. Created Kubernetes YAMLs

* `nodejs-app-deployment.yaml` using Docker Hub image

### 3. Applied Deployment & Service

```bash
kubectl apply -f nodejs-app-deployment.yaml
```

* Accessed app via LoadBalancer DNS URL

---

## CI/CD Pipeline with Jenkins

### 1. Setup Jenkins EC2

* Installed Jenkins, Docker, AWS CLI, kubectl, eksctl

### 2. Configured Jenkins Credentials

* DockerHub credentials
* AWS IAM credentials with EKS access

### 3. Jenkinsfile

* Declarative pipeline to automate:

  * Clone repo
  * Build & Push Docker image
  * Deploy to Kubernetes on EKS

* After build success: LoadBalancer DNS accessible via browser

---

## Files Structure

```
Eyego-Ai-Task/
├── index.js
├── package.json
├── Dockerfile
└── nodejs-app-deployment.yaml
```

---

## Output

Application accessible via:

```
http://<LoadBalancer-DNS-URL>
```
