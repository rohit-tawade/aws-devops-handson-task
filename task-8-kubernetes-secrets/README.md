# Task 8: Deploy a Container Using a Kubernetes Secret

## 🎯 Objective

Deploy a container using the image `hashicorp/http-echo` inside a Kubernetes namespace called `crecita-ns`.  
Inject a secret message into the container, and ensure the container displays the message on the browser.

---

## 🛠️ Steps Performed

### 1️⃣ Create a New Namespace

```bash
kubectl create namespace crecita-ns

# 2️⃣ Create Kubernetes Secret
- We created a secret named welcome-secret containing the message Welcome to Crecita.

```bash
kubectl create secret generic welcome-secret \
  --from-literal=WELCOME_MSG="Welcome to Crecita" \
  -n crecita-ns
```

### 3️⃣ Deployment YAML (http-echo-deployment.yaml)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: http-echo
  namespace: crecita-ns
spec:
  replicas: 1
  selector:
    matchLabels:
      app: http-echo
  template:
    metadata:
      labels:
        app: http-echo
    spec:
      containers:
        - name: http-echo
          image: hashicorp/http-echo
          args:
            - "-text=$(WELCOME_MSG)"
          env:
            - name: WELCOME_MSG
              valueFrom:
                secretKeyRef:
                  name: welcome-secret
                  key: WELCOME_MSG
          ports:
            - containerPort: 80
# 4️⃣ Service YAML (http-echo-service.yaml)
````yaml
apiVersion: v1
kind: Service
metadata:
  name: http-echo-service
  namespace: crecita-ns
spec:
  selector:
    app: http-echo
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
5️⃣ Apply Resources
bash
Copy
Edit
kubectl apply -f http-echo-deployment.yaml
kubectl apply -f http-echo-service.yaml
6️⃣ Port Forwarding to Access the App
bash
Copy
Edit
kubectl port-forward svc/http-echo-service 8080:80 -n crecita-ns
Now visit your browser:


http://localhost:8080
# ✅ Expected Output:
```plaintext
Welcome to Crecita

# ✅ Task Status
 Namespace created

 Secret injected securely (not hardcoded)

 Container runs and displays the secret message

 Verified output in browser using port-forward

# 📸 Screenshot

![Screenshot 1](<Screenshot 2025-06-18 022051.png>)

![Screenshot 2](<Screenshot 2025-06-18 022033.png>)

