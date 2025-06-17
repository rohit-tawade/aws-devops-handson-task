
````markdown
# Task 9: Deploy a Container Using a Kubernetes Secret (Auto-Refresh)

## 🎯 Objective

Deploy a container that:
- Reads a secret message from a Kubernetes Secret.
- Serves it using a simple web server.
- Automatically reflects changes to the secret in the browser (without pod restart).

---

## 🛠 Steps

### 1. Create Namespace

```bash
kubectl create namespace crecita-ns
````

---

### 2. Create Secret (with message)

```bash
kubectl create secret generic welcome-secret \
  --from-literal=message="🚀 Welcome to Crecita — Level Up DevOps!" \
  -n crecita-ns
```

✅ This creates a key called `message` in the secret `welcome-secret`.

---

### 3. Create Deployment (`http-server-secret.yaml`)

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: secret-web-server
  namespace: crecita-ns
spec:
  replicas: 1
  selector:
    matchLabels:
      app: secret-web-server
  template:
    metadata:
      labels:
        app: secret-web-server
    spec:
      containers:
        - name: web-server
          image: python:3.9-slim
          command: ["/bin/sh", "-c"]
          args:
            - |
              mkdir -p /usr/share/messages && \
              cp /etc/secret/message /usr/share/messages/index.html && \
              while true; do
                cp /etc/secret/message /usr/share/messages/index.html;
                sleep 30;
              done &
              cd /usr/share/messages && python3 -m http.server 80
          volumeMounts:
            - name: secret-vol
              mountPath: /etc/secret
              readOnly: true
      volumes:
        - name: secret-vol
          secret:
            secretName: welcome-secret
```

---

### 4. Create Service (`http-server-service.yaml`)

```yaml
apiVersion: v1
kind: Service
metadata:
  name: secret-web-service
  namespace: crecita-ns
spec:
  selector:
    app: secret-web-server
  ports:
    - protocol: TCP
      port: 80
      targetPort: 80
  type: ClusterIP
```

---

### 5. Apply Deployment and Service

```bash
kubectl apply -f http-server-secret.yaml
kubectl apply -f http-server-service.yaml
```

---

### 6. Access via Port Forwarding

```bash
kubectl port-forward svc/secret-web-service 8080:80 -n crecita-ns
```

Then open browser:

```text
http://localhost:8080
```

You should see:

```
🚀 Welcome to Crecita — Level Up DevOps!
```

---

### 7. Update Secret Without Restart

To update the message:

```bash
kubectl create secret generic welcome-secret \
  --from-literal=message="✅ Crecita DevOps Labs — Secrets Reload Live!" \
  -n crecita-ns --dry-run=client -o yaml | kubectl apply -f -
```

Wait up to 60 seconds, then refresh browser — message should update automatically.

---

## ✅ Expected Output

* Container auto-refreshes secret message
* Browser shows latest message without restarting pod

---

## 🔁 Verifiable Test

```bash
kubectl get secret welcome-secret -o yaml -n crecita-ns
```

```bash
curl http://localhost:8080
```

---

## 🧼 Cleanup

```bash
kubectl delete deployment secret-web-server -n crecita-ns
kubectl delete service secret-web-service -n crecita-ns
kubectl delete secret welcome-secret -n crecita-ns
```

---

## 📌 Notes

* Kubernetes updates mounted secrets within 60 seconds.
* The container reads the secret file periodically (every 30s).
* No need to restart the pod.

## 🔁 Screenshots

![Screenshot 1](<Screenshot 2025-06-18 023651.png>)

![Screenshot 2](<Screenshot 2025-06-18 023450.png>)
![Screenshot 3](<Screenshot 2025-06-18 023706.png>)
![Screenshot 4](<Screenshot 2025-06-18 023726.png>)
