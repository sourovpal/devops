# **Horizontal Pod Autoscaler (HPA)**

### 🧩 Step 1: Autoscale Command

```bash
  minikube addons enable metrics-server
  
  kubectl autoscale deployment <deployment-name> \
    --cpu-percent=50 \
    --min=2 \
    --max=10

  kubectl get hpa      # Show all hpa list
  kubectl get pods -w  # Live See pods up or down
  kubectl get hpa -w   # Live See hpa cup

  while true; do wget -q -O- http://html-website-service; done        # Random Traffic Generate for test
  ab -n 100000 -c 50 http://127.0.0.1:37655/

  kubectl get pods -n kube-system | grep metrics
```
📌 --cpu-percent=50% - যখন Over (51% - 100%) হবে তখনি নতুন Pod তৈরি হবে\
📌 --min=2 -  Min 2 টি Pod Run থাকবে বাকি গুলো traffic এর উপর ভিত্তি করে বাড়বে কমবে\
📌 --max=10 - Traffic বেশি হলে Max 10 Pod তৈরি হবে এর বেশি হবে না

📌 cpu: "1000m" = 1 Core\
📌 memory: "1024Mi" = 1GB | memory: "1Gi" = 1GB
