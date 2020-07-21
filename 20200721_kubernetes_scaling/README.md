## Kubernetes scaling

### Vocabulary

- Pod
    - Atomic unit in kubernetes
    - Consists of one of more containers
    - https://kubernetes.io/docs/concepts/workloads/pods/pod/
- Deployments
    - Collection of zero or more identical pods
    - https://kubernetes.io/docs/concepts/workloads/controllers/deployment/

### Example setup

- Assume there is a deployment called webapp
- Create one for example purposes: `kubectl create deployment webapp --image=micya/webapp:linux`
- The image is an empty ASP.NET Core website
- Check deployment: `kubectl get deployments`. It should say 1/1 replicas
- See pods in deployment: `kubectl get pods`

### Manual scaling

- To manually scale a deployment via commandline, use kubectl scale
- Example: `kubectl scale deployment webapp --replicas=3`
- Check that it worked by running `kubectl get deployment` and `kubectl get pods`

### Request and limits

### Metrics server


### Autoscaling

### Kubernetes event driven autoscaler (KEDA)