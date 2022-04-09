## Configure minikube to use local docker images
By default, Kubernetes tries to pull the images from public Docker registry. This means even for local testing, you will be required to push image to public Docker registry.

To prevent Kubernetes from pulling the image from public registry, 
- Set `imagePullPolicy` to `Never` in application deployment yaml file
- Point local Docker daemon to minikube internal Docker registry by running below command

```
eval $(minikube -p minikube docker-env)
```
**Note:** This command must be run in every new terminal.