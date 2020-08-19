 # Deployment
>Its a Controller like others ReplicationController or ReplicaSet but also helps to update and rollback with zero down time

**Features**
1. Multiple Replicas(default=1)
2. Upgrade
3. Rollback
4. Scale up and down
5. Pause and Resume

---
#### create a Deployment manifest file nginx-deploy.yml
>here the manifest file will be same as ReplicaSet, only difference will be: **kind: Deployment**

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deploy
  labels:
    app: nginx-app
spec:
  replicas: 3
  selector:
    matchLables:
      app: nginx-app
  template:  
  metadata:
    name: nginx-pod
    labels:
      app: nginx-app
    spec:
      containers:
        - name: nginx
          image: nginx
          imagePullPolicy: ifNotPresent
 ```

 - kubectl create -f nginx-deploy.yml


 **To display the deployments**
 - kubectl get deploy


 **To filter the deployments using lables**
 - kubectl get deploy -l app=nginx-app

>NOTE: when we create a deployment it will automatically create ReplicaSet

**To print complete details of the Deployment**
- kubectl describe deploy nginx-deploy

**To update the deployment from nginx 1.7.9 to 1.9.1**
>two possible ways using kubectl set or kubectl edit

- kubectl set image deploy nginx-deploy ngnix=nginx:1.9.1
or
- kubectl edit deploy nginx-deploy
>here the deployment file will open in vi editor so we can change the version and replica count                              


**To check the status of deployment**
- kubectl rollout status deployment/nginx-deployment
---

>Suppose we have  made some error while deploying instead of 1.9.1 we gave it as 1.91
in such case we have to rollback to previous stable deployment Version
- kubectl set image deploy ngix-deploy ngix-container=nginx:**1.91 --record**
To show the history related to this deployment
- kubectl rollout history deployment/nginx-deploy
Now we have to undo this rollout
- kubectl rollout undo deployment/nginx-deploy
now check the rollout status
-kubectl rollout status deployment/nginx-deploy


**To scale up the replica count**
- kubectl scale deployment nginx-deploy --replicas=5  


**To scale down the replica count**
- kubectl scale deployment nginx-deploy --replicas=1


**to delete the deployment**
-kubectl delete -f nginx-deploy.yml
>this will delete all the deployment,ReplicaSet, pods created as part of the manifest file.
