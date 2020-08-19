# Services
>A service is a  grouping of pods that are running on a cluster
lables should be same for the respective pods and services

#### Types of services
- **NodePort**
- **LoadBalancer**
- **ClusterIP**
---
---
---
## NodePort
>- exposes our webapp to the outside world
- let us assume we have  deployment app  and if we want to expose the app to outside world than we need a service of type NodePort.
- nodeport range 30000-32767

nodeport is the port on the node itself
port refers to the port on service
targetport refers to the port on the target container where actual app is running


#### Downsides of NodePort
1. you can only have once service per port
2. You can only use ports 30000 - 32767
3. If your node/VM IP address change, you need to deal with that


 **Create the deployment file nginx-deploy.yml**
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx-app
specs:
  replicas: 1
  selector:
    matchLabels:
      app: nginx-app
  template:
    metadata:
    labels:
      app: nginx-app
    specs:
      containers:
      - name: nginx-container
        image: nginx:1.7.9
        ports:
        - containerPort: 80
```



  **Create the Nodeport service file nginx-svc-np.yml**
```
apiVersion: v1
kind: Service
metadata:
  name: my-service
  labels:
    app: nginx-app
spec:
  selector:
    app:nginx-app
  type: NodePort
  ports:
    - nodePort: 31000
      port: 80
      targetPort: 80
```



*In service manifest file the selector section should be same as the pod label section*


**Create the deployment file**
- kubectl create -f nginx-deploy.yml


  **Create the nodeport service**
  - kubectl create -f nginx-svc-np.yml


  **To display the service**
  - kubectl get service -l app=nginx-app


  **To get complete details about the service**
  - kubectl describe svc my-service


  #### Now  we can access the app using:
  1. **Pod IP**
  - kubectl get pod -o wide
  >we will get the pod ip address here .
  so use curl http://pod_IP:targetport

  2. **Using service IP**
  - kubectl get svc -l app=nginx-app
  >we will get the service ip and the service port
  so use curl http://Service_IP:port_number

  3. **Using Node IP (external IP)**
 Find the external ip of node on which the pod is running
  - kubctl get pod -o wide (to check where the pod is running)

 Find the nodeport assigned to the service
 kubectl describe svc -l app=nginx-app
 >open in any browser
 NodeIP:NodePort/


 **To delete the files created we can simply delete the service we created and it will delete all the files related to that service**
 - kubectl delete svc my-service


---

## LoadBalancer
