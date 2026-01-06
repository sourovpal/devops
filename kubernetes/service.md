# **Service File Configration**

### 🧩 Related Commands
```bash
  kubectl apply -f <filename.yaml>
  kubectl get svc           # Show all runing service list
  minikube sercice <service-name>     # Open local server for dev
```


nodeport-service.yaml

### 🧩 NodePort Service Configration

```bash
apiVersion: v1
kind: Service
metadata:
  name: html-website-nodeport-service
spec:
  type: NodePort
  selector:
    app: html-website
  ports:
    - name: http
      protocol: TCP
      port: 80          # Service port
      targetPort: 80    # Pod port
      nodePort: 30080   # External access port (30000–32767)

```

### 🧩 LoadBalancer Service Configration

loadbalancer-service.yaml

```bash
apiVersion: v1
kind: Service
metadata:
  name: html-website-loadbalancer-service
spec:
  type: LoadBalancer
  selector:
    app: html-website
  ports:
    - name: http
      protocol: TCP
      port: 80
      targetPort: 80
```
