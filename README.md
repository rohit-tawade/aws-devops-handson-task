### 📄 `README.md` – Crecita DevOps Cloud Project (Simplified)

```markdown
# 🌐 Crecita DevOps Cloud Project

This project includes 9 hands-on DevOps tasks using tools like **Terraform, Ansible, Docker, Kubernetes, Prometheus, Grafana**, and **GitHub Actions**.

---

## ✅ Tasks Completed

### 1. Run React App Locally
- Installed dependencies and started the app using:
```

npm install
npm start

```


### 2. Provision EC2 Instance Using Terraform
- Wrote Terraform code to launch an EC2 instance with SSH (22) and HTTP (3000) access.
- Deployed using:
```

terraform init
terraform apply

```


### 3. Deploy React App using Ansible
- Created an Ansible playbook to:
- Install Node.js
- Clone the React app repo
- Run the app
- Connected to EC2 and deployed the app using the playbook.

---

### 4. Automate Deployment with GitHub Actions and Ansible
- Set up a CI/CD pipeline:
- GitHub Actions triggered Terraform to create EC2
- Then Ansible deployed the React app on it automatically

---

### 5. Docker + ECR + ECS Deployment
- Dockerized the React app and pushed the image to AWS ECR.
- Used Terraform to deploy it on ECS.
- App was accessible via Load Balancer.

---

### 6. Kubernetes Deployment
- Wrote Kubernetes Deployment and Service YAMLs for the React app.
- Deployed on Minikube using:
```

kubectl apply -f .

```

---

### 7. Monitor Kubernetes with Prometheus + Grafana
- Installed Prometheus and Grafana via Helm.
- Monitored pod CPU and memory usage in Grafana.
- Logged in using default credentials and explored dashboards.

---

### 8. Deploy Container Using Kubernetes Secret
- Created a Kubernetes Secret with a welcome message.
- Used the `hashicorp/http-echo` image.
- Injected the secret via environment variable.
- App showed the secret message via port-forward.

---

### 9. Auto-Refresh Secret in Container (No Pod Restart)
- Updated the secret message.
- Mounted the secret as a file (message.txt).
- Used a Python HTTP server to serve the file.
- Kubernetes auto-refreshed the secret within 60 seconds—no pod restart needed.

---

## 👨‍💻 Author

**Rohit Santram Tawade**  
DevOps Learner | AWS Cloud Enthusiast  
```