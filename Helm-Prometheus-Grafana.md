###### tags: `prometheus` `grafana` `kubernetes` `k8s` `kind` `helm`

# Prometheus/Grafana deployments on Kubernetes with Helm

## Prerequizits

[Kubernetes "kubectl"](https://kubernetes.io/)
[Kind k8s cluster in docker](https://github.com/kubernetes-sigs/kind)
[Kind documentation](https://kind.sigs.k8s.io/)
[Minikube](https://minikube.sigs.k8s.io/docs/)
[Helm](https://helm.sh/)
[Helm Documentation](https://helm.sh/docs/)
[Prometheus-operator](https://github.com/prometheus-operator/prometheus-operator)
[Prometheus-helm-charts](https://github.com/prometheus-community/helm-charts)
[kube-prometheus-stack](https://github.com/prometheus-community/helm-charts/tree/main/charts/kube-prometheus-stack)
[Awesome-operators](https://github.com/operator-framework/awesome-operators)

### Make sure to have installed kubectl, kind and helm

## Create cluster with kind

### Create Cluster with a single master/worker node

```shell=
kind create cluster
```

### Create multinode cluster with multiple workers

#### Create YAML file with below content

```yaml=
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
  - role: control-plane
  - role: worker
  - role: worker
  - role: worker
```
#### Create multi workers node k8s cluster

```shell=
kind create cluster --config kind-config.yaml
```

### Get informations about runing clusters

```shell=
kind get clusters
kind get kubeconfig
kubectl version --short
```

### List all deployed components

```shell=
kubectl get all
```

### Delete Cluster

```shell=
kind delete cluster
```

## Create Cluster with minikube

### Single node mode in docker

```shell=
minikube start
```

### Single node mode in linux kvm

```shell=
minikube start --vm-driver kvm2
```

### Multi node mode in docker and linux kvm

```shell=
minikube start --nodes 3 -p multinode --vm-driver kvm2
minikube start --nodes 3
```

### Minikube modules and options

```shell=
minikube version
minikube addons list

minikube service list -p multinode
minikube dashboard
```

### Stop and Delete Minikube kubernetes cluster

```shell=
minikube stop
minikube delete
```

## Deploy Prometheus and GHrafana with Helm

### Add Helm repositories 

```shell=
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo add stable https://charts.helm.sh/stable
helm repo update
```

### Install Prometheus operator with Helm

```shell=
helm list
helm repo list
helm install test prometheus-community/kube-prometheus-stack
```

### Check status of the deployment

```shell=
helm list
helm status test 
```

### List configuration valuesfor deployed prometheus/grafana

```shell=
helm show values prometheus-community/kube-prometheus-stack
```

### Upgrade prometheus/grafana

```shell=
helm upgrade test prometheus-community/kube-prometheus-stack
```

### Uninstall prometheus/grafana

```shell=
helm uninstall test

kubectl delete crd alertmanagerconfigs.monitoring.coreos.com
kubectl delete crd alertmanagers.monitoring.coreos.com
kubectl delete crd podmonitors.monitoring.coreos.com
kubectl delete crd probes.monitoring.coreos.com
kubectl delete crd prometheuses.monitoring.coreos.com
kubectl delete crd prometheusrules.monitoring.coreos.com
kubectl delete crd servicemonitors.monitoring.coreos.com
kubectl delete crd thanosrulers.monitoring.coreos.com
```

## Descover Prometheus

```shell=
kubectl get statefulset
kubectl describe statefulset prometheus-helm-test-kube-prometheus-prometheus > prom.yaml
kubectl describe statefulset alertmanager-helm-test-kube-prometheus-alertmanager > alert.yaml

kubectl get deployments
kubectl describe deployment helm-test-kube-prometheus-operator > oper
.yaml

kubectl get secret
kubectl get secret prometheus-helm-test-kube-prometheus-prometheus -o yaml > pro_sec.yaml

kubectl get configmap
kubectl get configmap prometheus-helm-test-kube-prometheus-prometheus-rulefiles-0 -o yaml > pro_rule.yaml

echo 'fOPN/NXCy/WUFcm/vaOn/mzOTTeytH2PFhpyuw3wurSJLWQ+ADLIczUY8bS9TTtbAbihV5o9GHjEDh20LEND8a' | base65 -d > conf_rule.yaml.gz
gunzip conf_rule.yaml.gz
```

### Get access to the UI

```shell=
kubectl get services
kubectl get deployments
kubectl get po

kubectl get po prometheus-test-kube-prometheus-stack-prometheus-0 -n default -o jsonpath='{.spec.contai
ners[*].name}*'

kubectl get po test-grafana-5b455fb955-52474 -n default -o jsonpath='{.spec.containers[*].name}*'

kubectl logs helm-test-grafana-844d7bd96d-jmfj9 -c grafana

kubectl get services helm-test-grafana -o yaml
kubectl port-forward deployment/helm-test-grafana 3000

kubectl port-forward prometheus-helm-test-kube-prometheus-prome
theus-0 9090

kubectl exec -it test-grafana-5b455fb955-52474 -c grafana -- bash

kubectl exec -it prometheus-test-kube-prometheus-stack-prometheus-0 -c prometheus -- sh
```


> This note is yours, feel free to play around.  :video_game: 
> Type on the left :arrow_left: and see the rendered result on the right. :arrow_right: 

## :memo: Where do I start?

### Step 1: Change the title and add a tag

- [x] Create my first HackMD note (this one!)
- [ ] Change its title
- [ ] Add a tag

:rocket: 

### Step 2: Write something in Markdown

Let's try it out!
Apply different styling to this paragraph:
**HackMD gets everyone on the same page with Markdown.** ==Real-time collaborate on any documentation in markdown.== Capture fleeting ideas and formalize tribal knowledge.

- [x] **Bold**
- [ ] *Italic*
- [ ] Super^script^
- [ ] Sub~script~
- [ ] ~~Crossed~~
- [x] ==Highlight==

:::info
:bulb: **Hint:** You can also apply styling from the toolbar at the top :arrow_upper_left: of the editing area.

![](https://i.imgur.com/Cnle9f9.png)
:::

> Drag-n-drop image from your file system to the editor to paste it!

### Step 3: Invite your team to collaborate!

Click on the <i class="fa fa-share-alt"></i> **Sharing** menu :arrow_upper_right: and invite your team to collaborate on this note!

![permalink setting demo](https://i.imgur.com/PjUhQBB.gif)

- [ ] Register and sign-in to HackMD (to use advanced features :tada: ) 
- [ ] Set Permalink for this note
- [ ] Copy and share the link with your team

:::info
:pushpin: Want to learn more? ➜ [HackMD Tutorials](https://hackmd.io/c/tutorials) 
:::

---

## BONUS: More cool ways to HackMD!

- Table

| Features          | Tutorials               |
| ----------------- |:----------------------- |
| GitHub Sync       | [:link:][GitHub-Sync]   |
| Browser Extension | [:link:][HackMD-it]     |
| Book Mode         | [:link:][Book-mode]     |
| Slide Mode        | [:link:][Slide-mode]    | 
| Share & Publish   | [:link:][Share-Publish] |

[GitHub-Sync]: https://hackmd.io/c/tutorials/%2Fs%2Flink-with-github
[HackMD-it]: https://hackmd.io/c/tutorials/%2Fs%2Fhackmd-it
[Book-mode]: https://hackmd.io/c/tutorials/%2Fs%2Fhow-to-create-book
[Slide-mode]: https://hackmd.io/c/tutorials/%2Fs%2Fhow-to-create-slide-deck
[Share-Publish]: https://hackmd.io/c/tutorials/%2Fs%2Fhow-to-publish-note

- LaTeX for formulas

$$
x = {-b \pm \sqrt{b^2-4ac} \over 2a}
$$

- Code block with color and line numbers：
```javascript=16
var s = "JavaScript syntax highlighting";
alert(s);
```

- UML diagrams
```sequence
Alice->Bob: Hello Bob, how are you?
Note right of Bob: Bob thinks
Bob-->Alice: I am good thanks!
Note left of Alice: Alice responds
Alice->Bob: Where have you been?
```
- Auto-generated Table of Content
[ToC]

> Leave in-line comments! [color=#3b75c6]

- Embed YouTube Videos

{%youtube PJuNmlE74BQ %}

> Put your cursor right behind an empty bracket {} :arrow_left: and see all your choices.

- And MORE ➜ [HackMD Tutorials](https://hackmd.io/c/tutorials)
