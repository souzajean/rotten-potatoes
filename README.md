# rotten-potatoes

## Configuração

MONGODB_DB => Nome do database

MONGODB_HOST => Host do MongoDB

MONGODB_PORT => Posta de acesso ao MongoDB

MONGODB_USERNAME => Usuário do MongoDB

MONGODB_PASSWORD => Senha do MongoDB



##Comandos

#VSCode
FROM python:3.8--slim-buster
WORKDIR /app
COPY requirements.txt .
RUN python -m pip install -r requirements.txt
COPY . /app
EXPOSE 5000
CMD ["gunicorn", "--workers=3", "--bind", "0.0.0.0:50000", "-c", "config.py", "app:app"]

#Terminal
docker build -t souzajean/rotten-potatoes:v1 .
docker image ls
docker login
docker push souzajean/rotten-potatoes:v1
docker tag souzajean/rotten-potatoes:v1 souzajean/rotten-potatoes:latest
docker image ls
docker push souzajean/rotten-potatoes:latest
docker container ls
k3d cluster create meucluster --servers 3 --agents 3 -p "8080:30000@loadbalancer"
docker container ls
kubectl get nodes
***VSCode***
#Deploy do MongoDB

apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongodb
spec:
  selector:
    matchLabels:
      app: mongodb
  template:
    metadata:
      labels:
        app: mongodb
    spec:
      containers:
        - name: mongodb
          image: mongo:5.0.5
          ports:
            - containerPort: 27017
          env: 
            - name: MONGO_INITDB_ROOT_USERNAME
              value: mongouser
            - name:  MONGO_INITDB_ROOT_PASSWORD
              value: mongopwd

---

apiVersion: v1
kind: Service
metadata:
  name: mongodb
spec:
  selector:
    app: mongodb
  ports:
  - port: 27017
  type: ClusterIP

***VSCode***


kubectl apply -f deployment.yaml
kubectl get pods
kubectl get service
kubectl get all

***VSCode***
#Deploy do MongoDB

apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongodb
spec:
  selector:
    matchLabels:
      app: mongodb
  template:
    metadata:
      labels:
        app: mongodb
    spec:
      containers:
        - name: mongodb
          image: mongo:5.0.5
          ports:
            - containerPort: 27017
          env: 
            - name: MONGO_INITDB_ROOT_USERNAME
              value: mongouser
            - name:  MONGO_INITDB_ROOT_PASSWORD
              value: mongopwd

---
#Service do MongoDB

apiVersion: v1
kind: Service
metadata:
  name: mongodb
spec:
  selector:
    app: mongodb
  ports:
  - port: 27017
  type: ClusterIP

---

#Deployment da aplicação web Rotten Potatoes

apiVersion: v1
kind: Deployment
metadata:
  name: web
  spec:
    selector:
      matchLabels:
        app: web
    template:
      metadata:
        labels:
        app: web
      spec:
        containers:
        - name: web
        image: souzajean/rotten-potatoes:v1
        ports:
        - containerPort: 5000
        env:
        - name: MONGODB_DB
          value: admin
        - name: MONGODB_HOST
          value: mongodb
        - name: MONGODB_PORT
          value: "27017"
        - name: MONGODB_USERNAME
          value: mongouser
        - name: MONGODB_PASSWORD
          value: mongopwd
        
---
# Services
apiVersion: v1
kind: Service
metadata:
  name: web
spec:
  selector:
    app: web
  ports:
  - port: 80
    targetPort: 5000
  type: NodePort
***VSCode***


kubectl apply -f deployment.yaml
kubectl get pods
kubectl get service
kubectl get all


