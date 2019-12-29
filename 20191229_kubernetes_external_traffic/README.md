There is a good [Medium post](https://medium.com/google-cloud/kubernetes-nodeport-vs-loadbalancer-vs-ingress-when-should-i-use-what-922f010849e0) detailing the differences between ClusterIP, NodePort, LoadBalancer, and Ingress in Kubernetes. These are my notes from testing each of them.

## Setup

For testing purposes, I created a blank ASP.NET core application, built a docker image, and pushed to the docker repository. The application looks like this:

![ASP.NET core default application](img/app.png)

I did most of my testing on an AKS cluster.

To deploy the application to my kubernetes cluster, I used the following yaml ([deployment.yaml](deployment.yaml)):

``` yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: webapplication1
spec:
  selector:
    matchLabels:
      app: webapplication1
  
  template:
    metadata:
      labels:
        app: webapplication1
    
    spec:
      containers:
      - name: webapplication1
  
        image: micya/webapplication1
        resources:
          limits:
            memory: "128Mi"
            cpu: "500m"
        ports:
        - containerPort: 80
```

Applied the yaml with the command

```
kubectl apply -f deployment.yaml
```

You should be able to see the deployment called "webapplication1" via `kubectl get deployments`.

Checking the pods, you should see one pod is up:

```
> kubectl get pods

NAME                               READY   STATUS    RESTARTS   AGE
webapplication1-5766f7ccc9-hs47q   1/1     Running   0          10m
```

## Port forwarding

The easiest way to access the application while testing is to do a simple port forwarding to the pod.

```
kubectl port-forward webapplication1-5766f7ccc9-hs47q 1234:80
```

In the above command, this maps a port (1234) on your local machine to a port (80) on the pod (webapplication1-5766f7ccc9-hs47q).

Navigating to http://localhost:1234/ in a browser, you should see the application as expected.

![ASP.NET core default application](img/app.png)

## ClusterIP

A ClusterIP is a service that does an internal mapping within kubernetes and allows other apps within the cluster to access your application. Note that this does NOT allow external access by default.

## NodePort

## LoadBalancer

## Ingress