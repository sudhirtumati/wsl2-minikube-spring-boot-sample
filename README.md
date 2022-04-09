Sample that demonstrates `spring-boot` application deployment to `minikube` cluster running on `WSL2`

## Prerequisites
- [Install minikube](MINIKUBE_INSTALL.md)
- [Configure Kubernetes to pull images from local registry](MINIKUBE_CONFIG.md)
  
## Build Docker image
```
mvn spring-boot:build-image
```

## Create Kubernetes deployment
```
kubectl apply -f kube/deployment.yaml
```

Run `kubectl get all` command to see all pods, services, and deployments. You should see output similar to the one below
```
NAME                       READY   STATUS    RESTARTS   AGE
pod/demo-ff7df4454-m4rsc   1/1     Running   0          4s

NAME                 TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
service/demo         LoadBalancer   10.105.70.44   <pending>     8080:31317/TCP   4s
service/kubernetes   ClusterIP      10.96.0.1      <none>        443/TCP          133m

NAME                   READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/demo   1/1     1            1           4s

NAME                             DESIRED   CURRENT   READY   AGE
replicaset.apps/demo-ff7df4454   1         1         1       4s
```

`EXTERNAL-IP` for service `demo` is `<pending>`. Due to this the application is not accessible from outside the cluster. To make application accessible outside the cluster, you need to run `minikube tunnel` in a new terminal. See [this link](https://minikube.sigs.k8s.io/docs/handbook/accessing/#using-minikube-tunnel) for more details.

After starting `minikube tunnel` in a separate terminal, you should see below output when you run `kubectl get all`
```
NAME                       READY   STATUS    RESTARTS   AGE
pod/demo-ff7df4454-m4rsc   1/1     Running   0          35m

NAME                 TYPE           CLUSTER-IP     EXTERNAL-IP   PORT(S)          AGE
service/demo         LoadBalancer   10.105.70.44   127.0.0.1     8080:31317/TCP   35m
service/kubernetes   ClusterIP      10.96.0.1      <none>        443/TCP          168m

NAME                   READY   UP-TO-DATE   AVAILABLE   AGE
deployment.apps/demo   1/1     1            1           35m

NAME                             DESIRED   CURRENT   READY   AGE
replicaset.apps/demo-ff7df4454   1         1         1       35m
```

Now an external ip (127.0.0.1 in this case) is assigned to the service. This makes application accessible from outside the cluster.

## Test the application
Running `curl 127.0.0.1:8080/actuator/health` command should produce below output
```
{"status":"UP","groups":["liveness","readiness"]}
```

Congratulations! You successfully installed `minikube` on `WSL2` and deployed a sample application.