# pod
> - collection of containers  
>- Each pod has its own ip address


##### create a manifests nginx-pod.yml file
```
apiVersion: v1
kind: Pod
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
- kubectl create -f nginx-pod.yml

**To list all pods inside kubernetes cluster**
- kubectl get pod

**To list all pods with the ip address and node assigned**
- kubectl get pod -o wide

**To show the yaml format of the pod created**
- kubectl get pod nginx-pod -0 yaml

**To show all details of the pod**
- kubectl describe pod nginx-pod

**To test if pod is working**
- ping < pod ip >
>ping 10.240.1.26

**To execute some commands on the pod**
- kubectl exec -it nginx-pod -- /bin/sh
>use exit command to exit out of the pod

**To delete the pod we created**
- kubectl delete pod nginx-pod
