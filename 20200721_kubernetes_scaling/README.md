## Kubernetes scaling

### Vocabulary

- Pod
    - Atomic unit in kubernetes
    - Consists of one of more containers
    - https://kubernetes.io/docs/concepts/workloads/pods/pod/
- Deployments
    - Collection of zero or more identical pods
    - https://kubernetes.io/docs/concepts/workloads/controllers/deployment/
- Pod autoscaling/horizontal pod autoscaler
    - Automatically scale the number of pods in a deployment based on metrics
    - https://kubernetes.io/docs/tasks/run-application/horizontal-pod-autoscale/
- Cluster autoscaling
    - Automatically increase the number of nodes (VMs) in a cluster based on metrics
    - Usually handled by the cloud vendor
    - AKS: https://docs.microsoft.com/en-us/azure/aks/cluster-autoscaler

### Example setup

Create an example deployment:

```
$ kubectl create deployment hpa-example --image=k8s.gcr.io/hpa-example
deployment.apps/hpa-example created
```

Check the deployment:

```
$ kubectl get deployments
NAME          READY   UP-TO-DATE   AVAILABLE   AGE
hpa-example   1/1     1            1           61s
```

Note Ready 1/1. This indicates that the deployment expects to have one replica, and one replica is currently ready.

Check the pods in the deployment:

```
$ kubectl get pods
NAME                           READY   STATUS    RESTARTS   AGE
hpa-example-68fb5754f6-9gm7m   1/1     Running   0          84s
```

As expected, there is one pod running.

### Manual scaling

Manually scale the deployment:

```
$ kubectl scale deployment hpa-example --replicas=3
deployment.apps/hpa-example scaled
```

Check the deployment:

```
$ kubectl get deployments
NAME          READY   UP-TO-DATE   AVAILABLE   AGE
hpa-example   3/3     3            3           3m49s
```

Check the pods in the deployment:

```
$ kubectl get pods
NAME                           READY   STATUS    RESTARTS   AGE
hpa-example-68fb5754f6-9gm7m   1/1     Running   0          3m51s
hpa-example-68fb5754f6-9q29f   1/1     Running   0          35s
hpa-example-68fb5754f6-q7fm4   1/1     Running   0          35s
```

### Request and limits

### Metrics server


### Autoscaling

Set up pod autoscaling:

```
$ kubectl autoscale deployment hpa-example --max=10
horizontalpodautoscaler.autoscaling/hpa-example autoscaled
```

There is a new resource called a horizontal pod autoscaler (HPA), which monitors the CPU usage relative to the pod CPU limit and autoscale threshold.

Check the HPA:

```
$ kubectl get hpa
NAME          REFERENCE                TARGETS         MINPODS   MAXPODS   REPLICAS   AGE
hpa-example   Deployment/hpa-example   <unknown>/80%   1         10        3          3m55s
```

### Kubernetes event driven autoscaler (KEDA)