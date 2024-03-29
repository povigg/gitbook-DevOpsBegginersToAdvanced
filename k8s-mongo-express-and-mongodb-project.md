# K8s mongo-express and mongoDB project

### Overview

Basic setup of web application and its database

<figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

### K8s components

<figure><img src=".gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>

### K8s: browser request flow

<figure><img src=".gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>

### Process

* Creating deployment file for mongodb
* Creating secret file which will store values  for username and password requried in deployment file.&#x20;
  * values need to be encoded, can do this from terminal:

```
echo -n 'username' | base64
dXNlcm5hbWU=

echo -n 'password' | base64
cGFzc3dvcmQ=
```

* Secret needs to be created before deployment: **kubectl apply -f mongo-secret.yaml**
* Apply the deployment **kubectl apply -f mongo-deployment.yaml**
* Create Service configuration.  Use the deployment.yaml file. --- will separate files within
* Create deployment file for mongo express
* Create external service to access mongo-express via browser. Use cmd: **minikube service mongo-express-service**&#x20;
