# k8s

```
system environment
Windows 10 Home + WSL2 + Docker Desktop
```



## minikube

minikube quickly sets up a local Kubernetes cluster on macOS, Linux, and Windows



### [Installation](https://minikube.sigs.k8s.io/docs/start/)



### Get started

```bash
# start minikube
$ minikube start

# start another minikube has profile name of hellok8s
$ minikube start -p hellok8s

# check profile list
$ minikube profile list

# check current profile
$ minikube profile

# switch profile to hellok8s
$ minikube profile hellok8s

# delete current profile
$ minikube delete
```



## kubectl

### Installation

```bash
$ curl -LO "https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl"
$ chmod +x ./kubectl
$ sudo mv ./kubectl /usr/local/bin/kubectl
```



### Get started

```bash
# check kubectl version
$ kubectl version

# wordpress-k8s.yml apply
$ kubectl apply -f wordpress-k8s.yml

#check current status
kubectl get all
```

```yaml
# wordpress-k8s.yml
# https://subicura.com/k8s/guide/

apiVersion: apps/v1
kind: Deployment
metadata:
  name: wordpress-mysql
  labels:
    app: wordpress
spec:
  selector:
    matchLabels:
      app: wordpress
      tier: mysql
  template:
    metadata:
      labels:
        app: wordpress
        tier: mysql
    spec:
      containers:
        - image: mysql:5.6
          name: mysql
          env:
            - name: MYSQL_ROOT_PASSWORD
              value: password
          ports:
            - containerPort: 3306
              name: mysql

---
apiVersion: v1
kind: Service
metadata:
  name: wordpress-mysql
  labels:
    app: wordpress
spec:
  ports:
    - port: 3306
  selector:
    app: wordpress
    tier: mysql

---
apiVersion: apps/v1
kind: Deployment
metadata:
  name: wordpress
  labels:
    app: wordpress
spec:
  selector:
    matchLabels:
      app: wordpress
      tier: frontend
  template:
    metadata:
      labels:
        app: wordpress
        tier: frontend
    spec:
      containers:
        - image: wordpress:5.5.3-apache
          name: wordpress
          env:
            - name: WORDPRESS_DB_HOST
              value: wordpress-mysql
            - name: WORDPRESS_DB_PASSWORD
              value: password
          ports:
            - containerPort: 80
              name: wordpress

---
apiVersion: v1
kind: Service
metadata:
  name: wordpress
  labels:
    app: wordpress
spec:
  type: NodePort
  ports:
    - port: 80
  selector:
    app: wordpress
    tier: frontend
```

```bash
# start wordpress service
$ minikube service wordpress
```

