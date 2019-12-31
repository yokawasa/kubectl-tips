# kubectl-tips
Tips on Kubernetes cluster management using kubectl command. A goal of this repository is to allow you reversely lookup kubectl commands from what you want to do around kubernetes cluster management.

<!-- TOC -->

- [kubectl-tips](#kubectl-tips)
    - [Print the supported API resources](#print-the-supported-api-resources)
    - [Print the available API versions](#print-the-available-api-versions)
    - [Deploy and rollback app using kubectl](#deploy-and-rollback-app-using-kubectl)
    - [Get all endpoints in the cluster](#get-all-endpoints-in-the-cluster)
    - [Execute shell commands inside the cluster](#execute-shell-commands-inside-the-cluster)
    - [Port forward a local port to a port on k8s resources](#port-forward-a-local-port-to-a-port-on-k8s-resources)
    - [Change the service type to LoadBalancer by patching](#change-the-service-type-to-loadbalancer-by-patching)
    - [Delete a worker node in the cluster](#delete-a-worker-node-in-the-cluster)
    - [Evicted all pods in a node for investigation](#evicted-all-pods-in-a-node-for-investigation)

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
kubectl rollout history deploy nginx
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
kubectl run busybox --restart=Never -it --image=busybox --rm /bin/sh
```
You can also exec shell commands in existing Pod
```bash
kubectl exec -n <namespace> -it <pod-name> -- /bin/sh
kubectl exec -n <namespace> -it <pod-name> -c <container-name> -- /bin/sh
```

> NOTE:
> You can change `kind` of the resource you're creating with option in `kubectl run`.
> | kind | option |
> | --- | --- |
> | deployment | none |
> | pod        | --restart=Never |
> | job        | --restart=OnFailure |
> | cronjob    | --schedule='cron format(0/5 * * * ?' |


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

