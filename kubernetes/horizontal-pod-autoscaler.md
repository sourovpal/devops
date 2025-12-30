# **Horizontal Pod Autoscaler (HPA)**

### 🧩 Step 1: Basic Configration

```bash
kubectl autoscale deployment <deployment-name> \
  --cpu-percent=50 \
  --min=2 \
  --max=10
```
📌 --cpu-percent=50 % যখন Over (51% - 100%) হবে তখনি নতুন Pod তৈরি হবে\
📌 --min=2 Min 2 টি Pod Run থাকবে বাকি গুলো traffic এর উপর ভিত্তি করে বাড়বে কমবে\
📌 --max=10 Traffic বেশি হলে Max 10 Pod তৈরি হবে এর বেশি হবে না
