# Replication Controller
 >ensures that a specified number of pods are running at any time
- if there are excess pods, they get killed and vice versa
- new pods are launched when they get fail, get deleted or terminated

> creating a rc with count of 1 ensures that a pod is always available

#### create  a manifest file for Replication Controller nginx-rc.yml

```
apiVersion: v1
kind: ReplicationController
metadata:
  name: nginx-rc
spec:
  replicas: 3
  selector:
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
- kubctl create -f nginx-rc.yml

**To filter the pods using lables**
- kubctl get pod -l app=nginx-app

**To display complete details of nginx ReplicationController**
 - kubectl describe rc nginx-rc    

**To scale up nginx app to 5 replicas**
- kubectl scale rc nginx-rc --replicas=5

**To scale down nginx app to 3 replicas**
- kubectl scale rc nginx-rc --replicas=3

**to delelte the ReplicationController created**
>if we delete the ReplicationController it will delete all the pods which are under its control.

- kubectl delete -f nginx-rc.yml
or
- kubectl delete rc nginx-rc

---
### Difference between ReplicationController and ReplicaSet
- ReplicaSet is next generation ReplicationController
- ReplicaSet support set based selector whereas ReplicationController supports equality based selector  
- equality based operators =, == , !=
>eg: environment = production
     tier != frontend
     ```
     kubctl get pods -l environment=production
     ```
- set based operators in, notin, exist
> has a advantage of having two or more options
eg: environment in (production, qa)
    tier notin (frontend,backend)
    ```
    kubectl get pods -l environmentin (production)
    ```

- Difference in  manifeast file
>
> supports on older resources such as: ReplicationController, services
 ```
selector:
    app: nginx
    tier: frontend
```
>---
supports on newer resources such as: ReplicaSet, Deployments, jobs, DaemonSet
```
selector:
      matchLables:
         app: nginx
         tier: frontend
```

---
# ReplicaSet

#### create a manifest file for ReplicaSet nginx-rs.yml

```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: nginx-rs
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
- kubectl create -f nginx-rs.yml

### Other commands are all same as ReplicationController only write rs instead of rc
