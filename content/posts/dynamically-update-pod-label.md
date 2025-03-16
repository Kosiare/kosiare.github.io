+++
title = "How to dynamically update pod label from the pod itself"
date = "2025-03-16"
tags = ["kubernetes", "kubernetes-api", "rbac"]
slug = "dynamically-update-pod-label-from-the-pod-itself"
summary = "In this post, I explain how I assigned unique labels to pods in a StatefulSet based on values known only to the init-container. I used the Downward API to expose pod metadata, but instead of using it directly, I leveraged curl and the Kubernetes API to add labels. This required setting up RBAC, a new ServiceAccount, and a RoleBinding to grant the necessary permissions."
+++

---
## Intro
Some time ago I came across a scenario where I needed to assign a label to a pod based on a value known only to the **init-container**. The pod was managed by a **ReplicaSet** within a **StatefulSet**, with between 2 and 10 replicas per cluster/environment. Each pod in the **StatefulSet** required a unique label value. The identifier for each pod was stored in **S3** storage, from which the **init-container** also downloaded additional data, including this identifier. These labels were later used in **Grafana** and **Loki** - **Prometheus** and **Promtail** scraped them, allowing me to build more insightful dashboards and create alerts for specific logs.

---
## Kubectl + Downward API?

The first thing that came to my mind was to simply pass **namespace** and **pod name** to container using <a href="https://kubernetes.io/docs/concepts/workloads/pods/downward-api/" target="_blank">**Downward API**</a> and then simply leverage `kubectl` inside the `entrypoint.sh` of **init-container**. 

<a href="https://kubernetes.io/docs/concepts/workloads/pods/downward-api/" target="_blank">**Downward API**</a> in Kubernetes allows pods to access their own metadata and resource information without needing to call the Kubernetes API. 
This metadata can be exposed to containers as environment variables or mounted as files in a volume. The name comes from the idea that Kubernetes control plane holds information "above" the pod. This allows the pod to access some of that information (name, namespace, annotations, labels, `resources.requests`, `resources.limits` and more).

It's worth noting - due to security and performance reasons, each pod can only access its own metadata and resource information - it cannot retrieve information about other resources.

In this case I needed to expose **pod name** and **namespace** to **init-container** as environment variables:

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: cool-statefulset
  namespace: cool-namespace
spec:
  template:
    spec:
    [...]
      containers:
      [...]
      initContainers:
      - name: cool-init-container
        env:
          - name: MY_POD_NAME
            valueFrom:
              fieldRef:
                fieldPath: metadata.name
          - name: MY_POD_NAMESPACE
            valueFrom:
              fieldRef:
                fieldPath: metadata.namespace
```

And then, inside `entrypoint.sh` of **init-cointainer** run:

```bash
$ kubectl label pod/$MY_POD_NAME -n $MY_POD_NAMESPACE "cool-label=$(cat /path/to/file/identifier)"
```

The command will expand to:

```bash
$ kubectl label pod/cool-statefulset-0 -n cool-namespace "cool-label=cool-identifier"
```

But after some thought I decided to approach it differently. *Why though? It'd work* - you'd ask. True but it requires to installing `kubectl` inside the **init-container** image which would add around **a few dozen megabytes** to image - not bad, not terrible but definitely not an optimal solution. Also I will have to update the version of `kubectl` from time to time - that's another hassle for the future to remember. And I don't like this kind of hassle therefore I had to pass on that idea.


---
## Kubernetes API + RBAC

I remembered that **init-container** base image was <a href="https://hub.docker.com/r/amazon/aws-cli" target="_blank">amazon/aws-cli</a> and it already had `curl` inside installed. I could use it to call **Kubernetes API** and add label to pod this way.
<br>Just like that:

```bash
curl -ik \
  -H "Accept: application/json, */*" \
  -H "Content-Type: application/strategic-merge-patch+json" \
  -H "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)"
  -X PATCH \
  -d '{"metadata":{"labels":{"cool-label":"cool-identifier"}}}' \
  https://kubernetes.default.svc.cluster.local/api/v1/namespaces/my-namespace/pods/cool-statefulset-0
```

#### *Eureka? Kinda.*
Solution above won't work. This call to **Kubernetes API** will fail due to a lack of permissions. The default **service account token** is not enough, it doesn't have any permissions to alter anything on the cluster. To make it work I need to apply proper **[RBAC](https://kubernetes.io/docs/reference/access-authn-authz/rbac/)** (*role-based access control*), of course using least-privilege approach:

#### 1. Role

Firstly I have to create a proper **Role** that will have privilege to add a label to a pod within its **namespace**.

```yaml
apiVersion: rbac.authorization.k8s.io/v1
kind: Role
metadata:
  name: cool-label-pod-role
  namespace: cool-namespace
rules:
  - apiGroups: ['']                   # "" indicates the "core" API group
    resources: ['pods']               # indicates resource name in API Group
    verbs: ['get', 'patch', 'list']   # indicates what operations are allowed
```

#### 2. A (*preferably new*) ServiceAccount

Then I have to create a new **ServiceAccount** that will use the **Role** from above.

```yaml
apiVersion: v1
kind: ServiceAccount
metadata:
  name: cool-service-account
  namespace: cool-namespace
```

#### 3. RoleBinding

And the last resource I need is a **RoleBinding**. As the name suggests - it'll bind a **Role** to **ServiceAccount**.

```yaml
kind: RoleBinding
apiVersion: rbac.authorization.k8s.io/v1
metadata:
  name: cool-role-biding
  namespace: cool-namespace
subjects:
- kind: ServiceAccount
  name: cool-service-account
  namespace: cool-namespace
roleRef:
  kind: Role
  name: cool-label-pod-role
  apiGroup: rbac.authorization.k8s.io
```


#### 4. Edit the StatefulSet

Since this is new **ServiceAccount**, I need to instruct **StatefulSet** to use it instead of default one.

```yaml
apiVersion: apps/v1
kind: StatefulSet
metadata:
  name: cool-statefulset
  namespace: cool-namespace
spec:
  template:
    spec:
    [...]
      serviceAccountName: cool-service-account
      containers:
      [...]
      initContainers:
      - name: cool-init-container
        env:
          - name: MY_POD_NAME
            valueFrom:
              fieldRef:
                fieldPath: metadata.name
          - name: MY_POD_NAMESPACE
            valueFrom:
              fieldRef:
                fieldPath: metadata.namespace
```

#### 5. Use env vars in API call

And finally I replaced `my-namespace` and `cool-statefulset-0` with proper variables inside `entrypoint.sh`.
<br>Of course I did it for label key and value as well but that depends on what you need to put in, mine source of label was a file inside **S3 bucket** in `key=value` format, which I properly evaluated earlier in `entrypoint.sh` of **init-container**.

```bash
curl -ik \
  -H "Accept: application/json, */*" \
  -H "Content-Type: application/strategic-merge-patch+json" \
  -H "Authorization: Bearer $(cat /var/run/secrets/kubernetes.io/serviceaccount/token)"
  -X PATCH \
  -d '{"metadata":{"labels":{"$LABEL_KEY":"$LABEL_VALUE"}}}' \
  https://kubernetes.default.svc.cluster.local/api/v1/namespaces/${MY_POD_NAMESPACE}/pods/${MY_POD_NAME}
```
