# 🌐 Spring Boot Login - Triển khai thủ công trên Google Kubernetes Engine (GKE) 

Ứng dụng Spring Boot tích hợp xác thực OAuth2, được đóng gói bằng Docker và triển khai thủ công lên GKE bằng `kubectl`.


---



## ✅ Tính năng chính 

 
- Xác thực OAuth2 với Auth0
 
- Container hóa bằng Docker
 
- Triển khai thủ công lên Google Kubernetes Engine
 
- Cấu hình LoadBalancer để truy cập công khai



---



## 🧰 Yêu cầu 

 
- Java 17
 
- Docker
 
- Tài khoản Google Cloud (GCP)
 
- Đã cài đặt:

 
  - [gcloud]()
 
  - [kubectl]()

---

## 🐳 Docker Build và Push 



```bash
docker build -t lxbach10012004/docker-example .
docker push lxbach10012004/docker-example:latest
```



---



## ☁️ Tạo Cluster GKE 

 
2. Vào **Kubernetes Engine**  trong GCP Console → Tạo cluster
 
4. Kết nối cluster:



```bash
gcloud container clusters get-credentials <cluster-name> --zone <zone> --project <project-id>
```



---



## 🚀 Triển khai ứng dụng 


### 1. Tạo Deployment 

`deployment.yaml`** 


```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: docker-example
spec:
  replicas: 1
  selector:
    matchLabels:
      app: docker-example
  template:
    metadata:
      labels:
        app: docker-example
    spec:
      containers:
        - name: docker-example
          image: lxbach10012004/docker-example:latest
          ports:
            - containerPort: 3000
```


### 2. Tạo Service 

`service.yaml`** 


```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-spring-boot-service
spec:
  selector:
    app: docker-example
  ports:
    - protocol: TCP
      port: 80
      targetPort: 3000
  type: LoadBalancer
```


### 3. Deploy 



```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
```



---



## 📸 Kiểm tra & minh chứng 


### 1. Kiểm tra tài nguyên Kubernetes 



```bash
kubectl get all
```

![Kiểm tra tài nguyên K8s](img_cloud/getall.png)





---



### 2. Truy cập ứng dụng qua External IP 



```bash
kubectl get service my-spring-boot-service
```

Sau khi có địa chỉ `EXTERNAL-IP`, mở trình duyệt và truy cập:


```cpp
http://<EXTERNAL-IP>
```

![Giao diện ứng dụng hoạt động](img_cloud/webui.png)


---



## 👤 Tác giả 

**Lê Xuân Bách**  – MSV 22024506