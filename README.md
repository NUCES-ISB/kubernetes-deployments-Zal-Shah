# Kubernetes Flask Messaging App 🚀

## 📌 Overview
This project deploys a **Flask-based messaging application** with a **PostgreSQL database** on a **Minikube-managed Kubernetes cluster**. The application allows users to store and retrieve messages via API calls while ensuring **persistent storage** and **scalability** using **Horizontal Pod Autoscaler (HPA).**

---

## 🔧 **Prerequisites**
Before deploying the app, ensure you have the following installed:
- [Minikube](https://minikube.sigs.k8s.io/docs/start/)
- [kubectl](https://kubernetes.io/docs/tasks/tools/)
- [Docker](https://www.docker.com/)
- Python 3.9+

---

## 🚀 **Deployment Steps**
### **1️⃣ Start Minikube**
```bash
minikube start --driver=docker
2️⃣ Deploy PostgreSQL
kubectl apply -f manifests/configmap/postgres-configmap.yaml
kubectl apply -f manifests/secret/postgres-secret.yaml
kubectl apply -f manifests/deployment/postgres-deployment.yaml
kubectl apply -f manifests/service/postgres-service.yaml
Verify PostgreSQL is running:

kubectl get pods -l app=postgres
3️⃣ Deploy Flask Application
kubectl apply -f manifests/deployment/flask-deployment.yaml
kubectl apply -f manifests/service/flask-service.yaml
Verify Flask App is running:

kubectl get pods -l app=flask-app
4️⃣ Enable Metrics Server & Deploy HPA
minikube addons enable metrics-server
kubectl apply -f manifests/deployment/flask-hpa.yaml
Verify HPA:

kubectl get hpa
🔬 Testing the Flask Application

1️⃣ Get the Service URL
minikube service flask-service --url
Example output:

http://192.168.49.2:31417
2️⃣ Add a Message
Visit:

http://192.168.49.2:31417/add?msg=Hello+World
Or use curl:

curl "http://192.168.49.2:31417/add?msg=Hello+World"
3️⃣ View Stored Messages
Visit:

http://192.168.49.2:31417/messages
Or use:

curl "http://192.168.49.2:31417/messages"
Expected output:

{
    "messages": ["Hello World\n"]
}
🔁 Testing Scaling & Persistence

1️⃣ Test Persistent Storage
Restart a Flask pod:

kubectl delete pod -l app=flask-app
Recheck messages:

curl "http://192.168.49.2:31417/messages"
✅ Messages should still be there!

2️⃣ Test Auto-Scaling
Simulate high CPU load:

kubectl run -it --rm load-generator --image=busybox -- /bin/sh
Inside the container, run:

while true; do wget -q -O- http://flask-service:5001/; done
Now, check HPA scaling:

kubectl get hpa
kubectl get pods
✅ HPA should scale pods up when CPU usage is high.

To stop the test, press CTRL + C and exit:

exit

📜 Final Git Push

git add .
git commit -m "Final Commit"
git push -u origin main
🎉 Congratulations! Your Kubernetes Deployment is Ready!

