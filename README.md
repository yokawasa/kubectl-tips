# kubectl-tips
Tips on Kubernetes cluster management using kubectl command. A goal of this repository is to allow you reversely lookup kubectl commands from what you want to do around kubernetes cluster management.

<!-- TOC -->

- [kubectl-tips](#kubectl-tips)
    - [Print the supported API resources](#print-the-supported-api-resources)
    - [Print the available API versions](#print-the-available-api-versions)
    - [Display Resource (CPU/Memory) usage of nodes/pods](#display-resource-cpumemory-usage-of-nodespods)
    - [Updating Kubernetes Deployments on a ConfigMap/Secrets Change](#updating-kubernetes-deployments-on-a-configmapsecrets-change)
    - [Deploy and rollback app using kubectl](#deploy-and-rollback-app-using-kubectl)
    - [Get all endpoints in the cluster](#get-all-endpoints-in-the-cluster)
    - [Execute shell commands inside the cluster](#execute-shell-commands-inside-the-cluster)
    - [Port forward a local port to a port on k8s resources](#port-forward-a-local-port-to-a-port-on-k8s-resources)
    - [Change the service type to LoadBalancer by patching](#change-the-service-type-to-loadbalancer-by-patching)
    - [Delete Kubernetes Resources](#delete-kubernetes-resources)
    - [Delete a worker node in the cluster](#delete-a-worker-node-in-the-cluster)
    - [Evicted all pods in a node for investigation](#evicted-all-pods-in-a-node-for-investigation)
    - [Get Pods Logs](#get-pods-logs)
    - [Get Kubernetes events](#get-kubernetes-events)
    - [Get Kubernetes Raw Metrics - Prometheus metrics endpoint](#get-kubernetes-raw-metrics---prometheus-metrics-endpoint)
    - [Get Kubernetes Raw Metrics - metrics API](#get-kubernetes-raw-metrics---metrics-api)

<!-- /TOC -->


## Print the supported API resources

```bash
kubectl api-resources
kubectl api-resources -o wide
```

<details><summary>sample output</summary>
<p>

```
NAME                              SHORTNAMES         APIGROUP                       NAMESPACED   KIND
bindings                                                                            true         Binding
componentstatuses                 cs                                                false        ComponentStatus
configmaps                        cm                                                true         ConfigMap
endpoints                         ep                                                true         Endpoints
events                            ev                                                true         Event
limitranges                       limits                                            true         LimitRange
namespaces                        ns                                                false        Namespace
nodes                             no                                                false        Node
persistentvolumeclaims            pvc                                               true         PersistentVolumeClaim
persistentvolumes                 pv                                                false        PersistentVolume
pods                              po                                                true         Pod
podtemplates                                                                        true         PodTemplate
replicationcontrollers            rc                                                true         ReplicationController
resourcequotas                    quota                                             true         ResourceQuota
secrets                                                                             true         Secret
serviceaccounts                   sa                                                true         ServiceAccount
services                          svc                                               true         Service
mutatingwebhookconfigurations                        admissionregistration.k8s.io   false        MutatingWebhookConfiguration
validatingwebhookconfigurations                      admissionregistration.k8s.io   false        ValidatingWebhookConfiguration
customresourcedefinitions         crd,crds           apiextensions.k8s.io           false        CustomResourceDefinition
apiservices                                          apiregistration.k8s.io         false        APIService
controllerrevisions                                  apps                           true         ControllerRevision
daemonsets                        ds                 apps                           true         DaemonSet
deployments                       deploy             apps                           true         Deployment
replicasets                       rs                 apps                           true         ReplicaSet
statefulsets                      sts                apps                           true         StatefulSet
applications                      app,apps           argoproj.io                    true         Application
appprojects                       appproj,appprojs   argoproj.io                    true         AppProject
tokenreviews                                         authentication.k8s.io          false        TokenReview
localsubjectaccessreviews                            authorization.k8s.io           true         LocalSubjectAccessReview
selfsubjectaccessreviews                             authorization.k8s.io           false        SelfSubjectAccessReview
selfsubjectrulesreviews                              authorization.k8s.io           false        SelfSubjectRulesReview
subjectaccessreviews                                 authorization.k8s.io           false        SubjectAccessReview
horizontalpodautoscalers          hpa                autoscaling                    true         HorizontalPodAutoscaler
cronjobs                          cj                 batch                          true         CronJob
jobs                                                 batch                          true         Job
certificatesigningrequests        csr                certificates.k8s.io            false        CertificateSigningRequest
leases                                               coordination.k8s.io            true         Lease
eniconfigs                                           crd.k8s.amazonaws.com          false        ENIConfig
events                            ev                 events.k8s.io                  true         Event
daemonsets                        ds                 extensions                     true         DaemonSet
deployments                       deploy             extensions                     true         Deployment
ingresses                         ing                extensions                     true         Ingress
networkpolicies                   netpol             extensions                     true         NetworkPolicy
podsecuritypolicies               psp                extensions                     false        PodSecurityPolicy
replicasets                       rs                 extensions                     true         ReplicaSet
networkpolicies                   netpol             networking.k8s.io              true         NetworkPolicy
poddisruptionbudgets              pdb                policy                         true         PodDisruptionBudget
podsecuritypolicies               psp                policy                         false        PodSecurityPolicy
clusterrolebindings                                  rbac.authorization.k8s.io      false        ClusterRoleBinding
clusterroles                                         rbac.authorization.k8s.io      false        ClusterRole
rolebindings                                         rbac.authorization.k8s.io      true         RoleBinding
roles                                                rbac.authorization.k8s.io      true         Role
priorityclasses                   pc                 scheduling.k8s.io              false        PriorityClass
storageclasses                    sc                 storage.k8s.io                 false        StorageClass
volumeattachments                                    storage.k8s.io                 false        VolumeAttachment

```

</p>
</details>

## Print the available API versions
```bash
kubectl get apiservices
```

<details><summary>sample output</summary>
<p>

```
NAME                                   SERVICE   AVAILABLE   AGE
v1.                                    Local     True        97d
v1.apps                                Local     True        97d
v1.authentication.k8s.io               Local     True        97d
v1.authorization.k8s.io                Local     True        97d
v1.autoscaling                         Local     True        97d
v1.batch                               Local     True        97d
v1.networking.k8s.io                   Local     True        97d
v1.rbac.authorization.k8s.io           Local     True        97d
v1.storage.k8s.io                      Local     True        97d
v1alpha1.argoproj.io                   Local     True        4d
v1alpha1.crd.k8s.amazonaws.com         Local     True        6d
v1beta1.admissionregistration.k8s.io   Local     True        97d
v1beta1.apiextensions.k8s.io           Local     True        97d
v1beta1.apps                           Local     True        97d
v1beta1.authentication.k8s.io          Local     True        97d
v1beta1.authorization.k8s.io           Local     True        97d
v1beta1.batch                          Local     True        97d
v1beta1.certificates.k8s.io            Local     True        97d
v1beta1.coordination.k8s.io            Local     True        97d
v1beta1.events.k8s.io                  Local     True        97d
v1beta1.extensions                     Local     True        97d
v1beta1.policy                         Local     True        97d
v1beta1.rbac.authorization.k8s.io      Local     True        97d
v1beta1.scheduling.k8s.io              Local     True        97d
v1beta1.storage.k8s.io                 Local     True        97d
v1beta2.apps                           Local     True        97d
v2beta1.autoscaling                    Local     True        97d
v2beta2.autoscaling                    Local     True        97d
```

</p>
</details>


## Display Resource (CPU/Memory) usage of nodes/pods


Display Resource (CPU/Memory) usage of nodes
```bash
kubectl top node
```

<details><summary>sample output</summary>
<p>

```
NAME                          CPU(cores)   CPU%   MEMORY(bytes)   MEMORY%
aks-node-28537427-0           281m         1%     10989Mi         39%
aks-node-28537427-1           123m         0%     6795Mi          24%
aks-node-28537427-2           234m         1%     7963Mi          28%
```
</p>
</details>


Display Resource (CPU/Memory) usage of pods
```bash
kubectl top po -A
kubectl top po --all-namespaces
```

<details><summary>sample output</summary>
<p>

```
NAMESPACE                   NAME                                           CPU(cores)   MEMORY(bytes)
dd-agent                    dd-agent-2nffb                                 55m          235Mi
dd-agent                    dd-agent-kkxsq                                 26m          208Mi
dd-agent                    dd-agent-srnlt                                 29m          210Mi
kube-system                 azure-cni-networkmonitor-5k7ws                 1m           22Mi
kube-system                 azure-cni-networkmonitor-72sxx                 1m           20Mi
kube-system                 azure-cni-networkmonitor-wxqvm                 1m           22Mi
kube-system                 azure-ip-masq-agent-gft8h                      1m           11Mi
kube-system                 azure-ip-masq-agent-tc8jc                      1m           10Mi
kube-system                 azure-ip-masq-agent-v54pm                      1m           11Mi
kube-system                 coredns-6cb457974f-kth9q                       4m           25Mi
kube-system                 coredns-6cb457974f-m9lth                       4m           24Mi
kube-system                 coredns-autoscaler-66cdbfb8fc-9kklp            1m           11Mi
kube-system                 kube-proxy-b4x7q                               5m           47Mi
kube-system                 kube-proxy-gm8df                               6m           49Mi
kube-system                 kube-proxy-vsgbs                               5m           50Mi
kube-system                 kubernetes-dashboard-686c6f85dc-n5xgg          1m           19Mi
kube-system                 metrics-server-5b9794db67-5rs25                1m           18Mi
kube-system                 tunnelfront-f586b8b5c-lrfkm                    68m          52Mi
custom-app-00-dev1          custom-app-deployment-5db9d949dd-fnrqd         2m           1563Mi
custom-app-00-dev2          custom-app-deployment-c67497d95-rtggx          2m           1577Mi
custom-app-00-dev3          custom-app-deployment-958c59798-6vvwj          2m           1581Mi
custom-app-00-dev4          custom-app-deployment-5c7797bc85-mlppp         2m           1585Mi
custom-app-00-dev5          custom-app-deployment-bfd4596dd-d5q76          2m           1603Mi
custom-app-00-dev6          custom-app-deployment-6f9f56ffc6-tpngg         2m           1600Mi
custom-app-00-dev7          custom-app-deployment-7c6896ff98-4pmln         2m           1573Mi
```

</p>
</details>


## Updating Kubernetes Deployments on a ConfigMap/Secrets Change

`kubectl v1.15+` provides a rollout restart command that allows you to restart Pods in a Deployment and allow them pick up changes to a referenced ConfigMap, Secret or similar.

```bash
kubectl rollout restart deploy/<deployment-name>
kubectl rollout restart deploy <deployment-name>
```


## Deploy and rollback app using kubectl
```bash
kubectl run nginx --image nginx
kubectl run nginx --image=nginx --port=80 --restart=Never
kubectl expose deployment nginx --external-ip="10.0.47.10" --port=8000 --target-port=80
kubectl scale --replicas=3 deployment nginx
kubectl set image deployment nginx nginx=nginx:1.8
kubectl rollout status deploy nginx
kubectl set image deployment nginx nginx=nginx:1.9
kubectl rollout status deploy nginx
kubectl rollout history deploy nginkubectl get --raw /metricsx
```

Let's check rollout history
```bash
kubectl rollout history deploy nginx

deployment.extensions/nginx
REVISION  CHANGE-CAUSE
1         <none>
2         <none>
3         <none>
```

You can undo the rollout of nginx deploy like this:
```bash
kubectl rollout undo deploy nginx
kubectl describe deploy nginx |grep Image
    Image:        nginx:1.8
```

More specifically you can undo the rollout with `--to-revision` option
```bash
kubectl rollout undo deploy nginx --to-revision=1
kubectl describe deploy nginx |grep Image
    Image:        nginx
```

## Get all endpoints in the cluster
```bash
kubectl get endpoints [-A|-n <namespace>]
kubectl get ep [-A|-n <namespace>]
```

## Execute shell commands inside the cluster
You can exec shell commands in a new creating Pod
```bash
kubectl run --generator=run-pod/v1 -it busybox --image=busybox --rm --restart=Never -- sh
```
If you want to run `curl` from a pod (The busybox image above doesn't contain curl)
```bash
kubectl run --generator=run-pod/v1 -it --rm busybox --image=radial/busyboxplus:curl --restart=Never -- sh
```
If you want to debug databases connections from a pod
```bash
kubectl run mysql -it --rm --image=mysql -- mysql -h <host/ip> -P <port> -u <user> -p<password>
```

You can also exec shell commands in existing Pod
```bash
kubectl exec -n <namespace> -it <pod-name> -- /bin/sh
kubectl exec -n <namespace> -it <pod-name> -c <container-name> -- /bin/sh
```

You can change `kind` of the resource you're creating with option in `kubectl run`

| Kind      | Option |
| ----------- | ----------- |
| deployment  | node       |
| pod         | `--restart=Never` |
| job         | `--restart=OnFailure` |
| cronjob     | `--schedule='cron format(0/5 * * * ?'` |


## Port forward a local port to a port on k8s resources

```bash
# kubectl port-forward -n <namespace> <resource> LocalPort:TargetPort
kubectl port-forward -n <namespace> redis-master-765d459796-258hz 7000:6379
kubectl port-forward -n <namespace> pods/redis-master-765d459796-258hz 7000:6379
kubectl port-forward -n <namespace> deployment/redis-master 7000:6379
kubectl port-forward -n <namespace> rs/redis-master 7000:6379
kubectl port-forward -n <namespace> svc/redis-master 7000:6379
```
See also [this](https://kubernetes.io/docs/tasks/access-application-cluster/port-forward-access-application-cluster/) for more detail

## Change the service type to LoadBalancer by patching 

```bash
# kubectl patch svc SERVICE_NAME -p '{"spec": {"type": "LoadBalancer"}}'
kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'
```

## Delete Kubernetes Resources

```bash
# Delete resources that has name=<label> label
kubectl delete svc,cm,secrets,deploy -l name=<label> -n <namespace>
# Delete all resources
kubectl delete svc,cm,secrets,deploy --all -n <namespace>

# Delete pods in namespace <namespace>
for pod in $(kubectl get po -n <namespace> --no-headers=true | cut -d ' ' -f 1); do
  kubectl delete pod $pod -n <namespace>
done

# Delete pods with --grace-period=0 and --force option
#    Add --grace-period=0 in order to delete pod as quickly as possible
#    Add --force in case that pod stay terminating state and cannnot be deleted
kubectl delete pod <pod> -n <namespace> --grace-period=0 --force
```

## Delete a worker node in the cluster

A point is to `cordon` a node at first, then to evict pods in the node with `drain`.
```bash
kubectl get nodes  # to get nodename to delete
kubectl cordon <node-name>
kubectl drain --ignore-daemonsets <node-name>
kubectl delete node <node-name>
```
> [NOTE] Add `--ignore-daemonsets` if you want to ignore DaemonSet for eviction

## Evicted all pods in a node for investigation

A point is to `cordon` a node at first, then to evict pods in the node with `drain`, to `uncordon` after the investigation
```bash
kubectl get nodes  # to get nodename to delete
kubectl cordon <node-name>
kubectl drain --ignore-daemonsets <node-name>
```
Now that you evicted all pods in the node, you can do investigation in the node. After the investigation, you can `uncordon` the node with the following command.
```
kubectl uncordon <node-name>
```
> [NOTE] Add `--ignore-daemonsets` if you want to ignore DaemonSet for eviction

## Get Pods Logs
You can get Kubernetes pods logs by running the following commands
```bash
kubectl logs <pod-name> -n <namespace>

# Add -f option if the logs should be streamed
kubectl logs <pod-name> -n <namespace> -f 
```

Or you can use 3rd party OSS tools. For example, you can get `stern` that allows you to aggregate logs of all pods and to filter them by using regular expression like `stern <expression>`
```bash
stern <keyword> -n <namespace>
```

## Get Kubernetes events 

You can Pods' events with the following commands which will show events at the end of the output for the pod (Relatively recent events will appear).

```bash
kubectl describe pod <podname> -n <namespace>
```

```bash
# Get recent events in a specific namespace
kubectl get events -n <namespaces>
kubectl get events -n kube-system

# Get recent events for all resources in the system
# Get events from all namespaces with either --all-namespaces or -A 
kubectl get event --all-namespaces
kubectl get event -A
kubectl get event -A -o wide

# No Pod events
kubectl get events --field-selector involvedObject.kind!=Pod
# Events from a specific Pod
kubectl get events --field-selector involvedObject.kind=Pod,involvedObject.name=<podname>
# Events from a specific Node
kubectl get events --field-selector involvedObject.kind=Node,involvedObject.name=<nodename>

# Warning events (from all namespaces)
kubectl get events --field-selector type=Warning -A
# NOT normal events (from all namespaces)
kubectl get events --field-selector type!=Normal -A
```

## Get Kubernetes Raw Metrics - Prometheus metrics endpoint

Many Kubernetes components exposes their metrics via the `/metrics` endpoint, including API server, etcd and many other add-ons. These metrics are in [Prometheus format](https://github.com/prometheus/docs/blob/master/content/docs/instrumenting/exposition_formats.md), and can be defined and exposed using [Prometheus client libs](https://prometheus.io/docs/instrumenting/clientlibs/)

```
kubectl get --raw /metrics
```

<details><summary>sample output</summary>
<p>

```
APIServiceOpenAPIAggregationControllerQueue1_adds 282663
APIServiceOpenAPIAggregationControllerQueue1_depth 0
APIServiceOpenAPIAggregationControllerQueue1_longest_running_processor_microseconds 0
APIServiceOpenAPIAggregationControllerQueue1_queue_latency{quantile="0.5"} 63
APIServiceOpenAPIAggregationControllerQueue1_queue_latency{quantile="0.9"} 105
APIServiceOpenAPIAggregationControllerQueue1_queue_latency{quantile="0.99"} 126
APIServiceOpenAPIAggregationControllerQueue1_queue_latency_sum 1.4331448e+07
APIServiceOpenAPIAggregationControllerQueue1_queue_latency_count 282663
APIServiceOpenAPIAggregationControllerQueue1_retries 282861
APIServiceOpenAPIAggregationControllerQueue1_unfinished_work_seconds 0
APIServiceOpenAPIAggregationControllerQueue1_work_duration{quantile="0.5"} 59
APIServiceOpenAPIAggregationControllerQueue1_work_duration{quantile="0.9"} 98
APIServiceOpenAPIAggregationControllerQueue1_work_duration{quantile="0.99"} 2003
APIServiceOpenAPIAggregationControllerQueue1_work_duration_sum 2.1373689e+07
APIServiceOpenAPIAggregationControllerQueue1_work_duration_count 282663
...
workqueue_work_duration_seconds_bucket{name="non_structural_schema_condition_controller",le="1e-08"} 0
workqueue_work_duration_seconds_bucket{name="non_structural_schema_condition_controller",le="1e-07"} 0
workqueue_work_duration_seconds_bucket{name="non_structural_schema_condition_controller",le="1e-06"} 0
workqueue_work_duration_seconds_bucket{name="non_structural_schema_condition_controller",le="9.999999999999999e-06"} 459
workqueue_work_duration_seconds_bucket{name="non_structural_schema_condition_controller",le="9.999999999999999e-05"} 473
workqueue_work_duration_seconds_bucket{name="non_structural_schema_condition_controller",le="0.001"} 924
workqueue_work_duration_seconds_bucket{name="non_structural_schema_condition_controller",le="0.01"} 927
workqueue_work_duration_seconds_bucket{name="non_structural_schema_condition_controller",le="0.1"} 928
workqueue_work_duration_seconds_bucket{name="non_structural_schema_condition_controller",le="1"} 928
workqueue_work_duration_seconds_bucket{name="non_structural_schema_condition_controller",le="10"} 928
workqueue_work_duration_seconds_bucket{name="non_structural_schema_condition_controller",le="+Inf"} 928
workqueue_work_duration_seconds_sum{name="non_structural_schema_condition_controller"} 0.09712353499999991
...
```
</p>
</details>


## Get Kubernetes Raw Metrics - metrics API

You can access [Metrics API](https://github.com/kubernetes/metrics) via kubectl proxy like this:

```
kubectl get --raw /apis/metrics.k8s.io/v1beta1/nodes
kubectl get --raw /apis/metrics.k8s.io/v1beta1/pods
kubectl get --raw /apis/metrics.k8s.io/v1beta1/nodes/<node-name>
kubectl get --raw /apis/metrics.k8s.io/v1beta1/namespaces/<namespace-name>/pods/<pod-name>
```
(ref: [feiskyer/kubernetes-handbook](https://github.com/feiskyer/kubernetes-handbook/blob/master/en/addons/metrics.md#metrics-api))

Outputs are unformated JSON. It's good to use `jq` to parse it.

<details><summary>sample output -  /apis/metrics.k8s.io/v1beta1/nodes</summary>
<p>

```json
kubectl get --raw /apis/metrics.k8s.io/v1beta1/nodes | jq

{
  "kind": "NodeMetricsList",
  "apiVersion": "metrics.k8s.io/v1beta1",
  "metadata": {
    "selfLink": "/apis/metrics.k8s.io/v1beta1/nodes"
  },
  "items": [
    {
      "metadata": {
        "name": "ip-xxxxxxxxxx.ap-northeast-1.compute.internal",
        "selfLink": "/apis/metrics.k8s.io/v1beta1/nodes/ip-xxxxxxxxxx.ap-northeast-1.compute.internal",
        "creationTimestamp": "2020-05-24T01:29:05Z"
      },
      "timestamp": "2020-05-24T01:28:58Z",
      "window": "30s",
      "usage": {
        "cpu": "105698348n",
        "memory": "819184Ki"
      }
    },
    {
      "metadata": {
        "name": "ip-yyyyyyyyyy.ap-northeast-1.compute.internal",
        "selfLink": "/apis/metrics.k8s.io/v1beta1/nodes/ip-yyyyyyyyyy.ap-northeast-1.compute.internal",
        "creationTimestamp": "2020-05-24T01:29:05Z"
      },
      "timestamp": "2020-05-24T01:29:01Z",
      "window": "30s",
      "usage": {
        "cpu": "71606060n",
        "memory": "678944Ki"
      }
    }
  ]
}

```

</p>
</details>


<details><summary>sample output - /apis/metrics.k8s.io/v1beta1/namespaces/NAMESPACE/pods/PODNAME </summary>
<p>

```json
kubectl get --raw /apis/metrics.k8s.io/v1beta1/namespaces/kube-system/pods/cluster-autoscaler-7d8d69668c-5rcmt | jq

{
  "kind": "PodMetrics",
  "apiVersion": "metrics.k8s.io/v1beta1",
  "metadata": {
    "name": "cluster-autoscaler-7d8d69668c-5rcmt",
    "namespace": "kube-system",
    "selfLink": "/apis/metrics.k8s.io/v1beta1/namespaces/kube-system/pods/cluster-autoscaler-7d8d69668c-5rcmt",
    "creationTimestamp": "2020-05-24T01:33:17Z"
  },
  "timestamp": "2020-05-24T01:32:58Z",
  "window": "30s",
  "containers": [
    {
      "name": "cluster-autoscaler",
      "usage": {
        "cpu": "1030416n",
        "memory": "27784Ki"
      }
    }
  ]
}

```
</p>
</details>

