# Jenkins-On-Minikube



install minikube on ubuntu system 


curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64

sudo install minikube-linux-amd64 /usr/local/bin/minikube

After install start minikube 
minikube start


alias kubectl="minikube kubectl --"

kreate name space 
kubectl create namespace jenkins


Create a jenkins-deployment.yaml

apiVersion: apps/v1
kind: Deployment
metadata:
  name: jenkins #name of the Jenkins Kubernetes Deployment
spec:
  replicas: 1 #number of Jenkins pods created in the Minikube Kubernetes Cluster
  selector:
    matchLabels:
      app: jenkins
  template:
    metadata:
      labels:
        app: jenkins
    spec:
      containers:
      - name: jenkins #name of the Jenkins container
        image: jenkins/jenkins:lts-jdk11 #name of the Jenkins image used to create the Jenkins container
        ports:
        - containerPort: 8080 #the port in which the Jenkins container will run inside the Minikube Kubernetes Cluster



#Create a Jenkins Kubernetes Deployment in the `jenkins` namespace
kubectl apply -f jenkins-deployment.yaml --namespace jenkins

kubectl get deployment --namespace jenkins

kubectl get pod --namespace jenkins

Create a jenkins-service.yaml

apiVersion: v1
kind: Service
metadata:
  name: jenkins #name of the Jenkins Kubernetes Service
spec:
  type: NodePort #Type of the Jenkins Kubernetes Service
  ports:
  - port: 8080 #port for the Jenkins Kubernetes Service
    targetPort: 8080 #port for the Jenkins pod
  selector:
    app: jenkins

#Create a Jenkins Kubernetes Service in the `jenkins` namespace
kubectl apply -f jenkins-service.yaml --namespace jenkins

Get service detail like service name , port
kubectl get service --namespace jenkins

get minikube ip 
minikube ip


perform ssh turnning to acsess jenkins on local machine
ssh -i "akash-devops.pem" -L <local port>:<minikube ip>:<service NodePort> ubuntu@65.2.153.191
ssh -i "akash-devops.pem" -L 8080:192.168.49.2:31401 ubuntu@65.2.153.191
