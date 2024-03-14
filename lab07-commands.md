# To Start

```
minikube start
```

# Part 1

## Step 1

cd 1-pods/

```
kubectl apply -f pods01.yaml
```

## Step 2

```
kubectl get pods
```

## Step 3

```
kubectl get pods -o wide
```

```
kubectl get pods -o json
```

## Step 4

```
kubectl exec -it webserver -- /bin/bash
```

## Step 5

In root@webserver:/# run the command

```
cat /etc/os-release
```

Then exit the shell

```
exit
```

## Step 6

```
kubectl logs webserver
```

## Step 7

```
kubectl delete -f pods01.yaml
```

```
kubectl get po -o wide
```

## Step 8

```
kubectl apply -f pods02.yaml
```

```
kubectl get po -o wide
```

```
kubectl get po,svc,deploy
```

## Step 9

```
kubectl describe pod webserver
```

## Step 10

```
kubectl exec -it webserver -c webwatcher -- /bin/bash
```

Run command in shell

```
cat etc/hosts
```

Then exit shell

```
exit
```

## Step 11

```
kubectl delete -f pods02.yaml
```

## Step 12

```
kubectl apply -f pods03.yaml
```

## Step 13

```
kubectl get po,svc
```

## Step 14

```
kubectl exec mc1 -c 1st -- /bin/cat /usr/share/nginx/html/index.html
```

```
kubectl exec mc1 -c 2nd -- /bin/cat /html/index.html
```

## Step 15

```
kubectl delete -f pods03.yaml
```

# Part 2

## Step 1

cd 2-replicasets/

```
kubectl apply -f nginx_replicaset.yaml
```

```
kubectl get rs
```

## Step 2

```
kubectl get po
```

Choose one of the names from above

```
kubectl get pods <chosen-name> -o yaml | grep -A 5 owner
```

## Step 3

```
kubectl edit pods <chosen-name>
```

Chosen name from step 2
This opens a vim terminal change label: role: web &rarr; isolated
To exit vim: ESC then :wq

## Step 4

```
kubectl scale --replicas=6 -f nginx_replicaset.yaml
```

## Step 5

```
kubectl autoscale rs web --max=5
```

## Step 6

Create orphan.yaml with the following code

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: orphan
  labels:
    role: web
spec:
  containers:
    - name: orphan
      image: httpd
```

Then run the command

```
kubectl delete -f nginx_replicaset.yaml
```

```
kubectl apply -f orphan.yaml
```

```
kubectl apply -f nginx_replicaset.yaml
```

```
kubectl get po
```

## Step 7

```
kubectl delete pods orphan
```

```
kubectl get pods
```

# Part 3

## Step 1

cd 3-deployments/

```
kubectl create -f nginx-dep.yaml
```

## Step 2

```
kubectl get deployments
```

```
kubectl describe deploy
```

## Step 3

```
kubectl get deployments
```

```
kubectl get po
```

## Step 4

```
kubectl scale deployments/nginx-deployment --replicas=4
```

```
kubectl get deployments
```

## Step 5

```
kubectl describe deployments/nginx-deployment
```

```
kubectl get pods -o wide
```

## Step 6

```
kubectl scale deployments/nginx-deployment --replicas=2
```

```
kubectl get deployments
```

## Step 7

```
kubectl set image deployments/nginx-deployment nginx=nginx:1.9.1
```

```
kubectl get pods -w
```

Press CTRL+C to terminate watching the pods

## Step 8

```
kubectl describe pods
```

## Step 9

```
kubectl rollout undo deployments/nginx-deployment
```

```
kubectl rollout status deployments/nginx-deployment
```

## Step 10

```
kubectl describe deployments | grep Strategy
```

# Part 4

## Step 1

cd 4-services/

```
kubectl apply -f nginx-svc.yaml
```

## Step 2

```
kubectl get svc my-nginx
```

```
kubectl describe svc my-nginx
```

## Step 3

Get a deployment using

```
kubectl get po
```

then run the command

```
kubectl exec <Chosen-deployment> -- printenv | grep SERVICE
```

## Step 4

```
kubectl scale deployment nginx-deployment --replicas=0
```

```
kubectl scale deployment nginx-deployment --replicas=2
```

## Step 5

```
kubectl get pods -l run=my-nginx -o wide
```

## Step 6

Select one of the deployments show in step 5 or run kubectl get po

then run the command

```
kubectl exec <chosen-deployment> -- printenv | grep SERVICE
```

## Step 7

```
kubectl get services kube-dns --namespace=kube-system
```

## Step 8

```
kubectl run curl --image=radial/busyboxplus:curl -i --tty
```

## Step 9

Once in the shell run this command

```
nslookup my-nginx
```

then exit shell

```
exit
```

## Step 10

```
kubectl apply -f nginx-svc-nodeport.yaml
```

```
kubectl get svc my-nginx -o yaml | grep nodePort -C 5
```

## Step 11

```
kubectl get svc my-nginx
```

## Step 12

```
kubectl get nodes -o yaml | grep addresses -C 1
```

```
curl http://192.168.49.2:31704 -k
```

# Part 5

## Step 1

cd 5-ingress/

```
minikube addons enable ingress
```

## Step 2

```
kubectl get pods --all-namespaces -l app.kubernetes.io/name=ingress-nginx
```

## Step 3

```
kubectl apply -f apple.yaml
```

```
kubectl apply -f banana.yaml
```

## Step 4

```
kubectl create -f ingress.yaml
```

## Step 5

```
minikube ip
```

## Step 6

```
curl -kL http://<Minikube IP>/apple
```

```
curl -kL http://<Minikube IP>/banana
```

```
curl -kL http://<Minikube IP>/notfound
```

# Part 6

## Step 1

cd 6-rbac/

```
kubectl apply -f service-account.yaml
```

## Step 2

```
kubectl apply -f role.yaml
```

## Step 3

```
kubectl apply -f role-binding.yaml
```

## Step 4

```
kubectl apply -f pods01.yaml
```

## Step 5

```
kubectl describe pod webserver
```

# Stop minikube
```
minikube stop
```
