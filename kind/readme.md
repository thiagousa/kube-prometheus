# Kubernetes 1.24 Monitoring Guide - KUBE - PROMETHEUS

![Screenshot](architecture-min.png)

![Screenshot](/kind/prometheus.png)

## KIND

Create a cluster with [kind](https://kind.sigs.k8s.io/docs/user/quick-start/)

```
cd kind

kind create cluster --name monitoring --image kindest/node:v1.24.1 --config kind.yaml

kubectl cluster-info --context kind-monitoring

kubectl config set-context kind-monitoring

```



Test our cluster to see all nodes are healthy and ready:

```
kubectl get nodes
NAME                       STATUS   ROLES           AGE   VERSION
monitoring-control-plane   Ready    control-plane   47s   v1.24.1
monitoring-worker          Ready    <none>          26s   v1.24.1
monitoring-worker2         Ready    <none>          26s   v1.24.1
monitoring-worker3         Ready    <none>          26s   v1.24.1
```

# Kube Prometheus

# Release schedule

Kube-prometheus has a somehow predictable release schedule, releases were
historically cut in sync with OpenShift releases as per downstream needs. So
far there hasn't been any problem with this schedule since it is also in sync
with Kubernetes releases. So for every new Kubernetes release, there is a new
release of kube-prometheus, although it tends to happen later.


The best method for monitoring, is to use the community manifests on the `kube-prometheus`
repository [here](https://github.com/prometheus-operator/kube-prometheus)

Now according to the compatibility matrix, we will need `release-0.10` to be compatible with
Kubernetes 1.24. </br>

# Setup CRDs

Let's create the CRD's and prometheus operator - [here](https://kubernetes.io/docs/concepts/extend-kubernetes/api-extension/custom-resources/)

```
kubectl create -f ./manifests/setup/
```

# Setup Manifests

Apply rest of manifests

```
kubectl create -f ./manifests/
```

# Check Monitoring

```
kubectl -n monitoring get pods

NAME                                   READY   STATUS    RESTARTS   AGE
alertmanager-main-0                    2/2     Running   0          68m
alertmanager-main-1                    2/2     Running   0          68m
alertmanager-main-2                    2/2     Running   0          68m
grafana-546559f668-v5z9c               1/1     Running   0          68m
kube-state-metrics-576b75c6f7-dhbhw    3/3     Running   0          68m
node-exporter-2744m                    2/2     Running   0          68m
node-exporter-kmcpg                    2/2     Running   0          68m
node-exporter-l5x7h                    2/2     Running   0          68m
node-exporter-xg4gp                    2/2     Running   0          68m
prometheus-adapter-5f68766c85-m64qc    1/1     Running   0          68m
prometheus-adapter-5f68766c85-r8754    1/1     Running   0          68m
prometheus-k8s-0                       2/2     Running   0          68m
prometheus-k8s-1                       2/2     Running   0          68m
prometheus-operator-79c5847fd8-zx596   2/2     Running   0          68m

```

# View Dashboards 

You can access the dashboards by using `port-forward` to access Grafana.
It does not have a public endpoint for security reasons

```
kubectl -n monitoring port-forward svc/grafana 3000
```

Then access Grafana on [localhost:3000](http://localhost:3000/)

## Check Prometheus 

Similar to checking Grafana, we can also check Prometheus:

```
kubectl -n monitoring port-forward svc/prometheus-operated 9090
```



## Check Service Monitors 

To see how Prometheus is configured on what to scrape , we list service monitors

```
kubectl -n monitoring get servicemonitors
NAME                      AGE
alertmanager-main         4m41s
coredns                   4m41s
grafana                   4m41s
kube-apiserver            4m41s
kube-controller-manager   4m41s
kube-scheduler            4m41s
kube-state-metrics        4m41s
kubelet                   4m41s
node-exporter             4m41s
prometheus-adapter        4m41s
prometheus-k8s            4m41s
prometheus-operator       4m40s

kubectl -n monitoring describe servicemonitor node-exporter
```

Label selectors are used to map service monitor to kubernetes services. </br>

````
kubectl get svc -n monitoring node-exporter --show-labels

````

That is how Prometheus is configured on what to scrape.

# Alert Manager 
````
kubectl -n monitoring port-forward svc/alertmanager-main 9093
````
# Delete Kind

````
kind delete clusters monitoring    
````