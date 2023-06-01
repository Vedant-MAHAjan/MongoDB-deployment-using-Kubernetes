# Database deployment using Kubernetes
Deploy MongoDB and Mongo-Express applications using Kubernetes

## Agenda

📋 Consider deploying 2 apps, MongoDB and Mongo-Express

🔗 MongoDB will be connected via an internal service

🔐 MongoDB URL is stored in a ConfigMap

🔑 MongoDB username and password are stored in secrets

🌐 To access Mongo-Express through the browser, an external service will be created

## Request Flow from browser to pod

1. 🌐 Browser requests to mongo-express external service
2. ⚡ External service requests to the mongo-express pod
3. 🚀 Pod will request the MongoDB internal service
4. 🔄 Internal service will request the MongoDB pod

## Order of execution

### 1. Secrets file for the mongodb pod

💡 This file will hold the username and password

💡 These values are stored in base64 encoded form

```
kubectl apply -f mongo-secret.yaml
```

#### Note: In this case, the secrets file is stored in this same repository for simplification.

### 2. Deployment file for mongodb

📦 Base image will be mongo

🔌 Port number will be exposed

🔧 Env will contain the root username and password

🔑 The values will be fetched from the secret

```
kubectl apply -f mongo.yaml
```

### 3. Internal service for the mongodb pod

🎯 Selector to connect to the pod through a label

🔗 Internal port and target port mentioned

#### Note: The deployment and service configurations are present in the same YAML file

### 4. ConfigMap for the mongo-express 

🔧 Contains the database URL for the MongoDB server

```
kubectl apply -f mongodb-config.yaml
```

### 5. Deployment file for mongo-express

📦 Specs will have the name of the container

🖼️ Name of the image

🔌 Details of the ports exposed

🔧 Env with the username and password of MongoDB

```
kubectl apply -f mongo-express.yaml
```

### 6. Extrernal service for mongo-express

🌐 Helps access from the browser

⚙️ The type section in the spec will be LoadBalancer

#### Note: The deployment and external service are present in the same YAML file
