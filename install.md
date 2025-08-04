# Installation Notes


1. Install was successfull

```
selvik@Selvis-MacBook-Pro quickstart % ./llmd-infra-installer.sh --namespace ${NAMESPACE} -r infra-inference-scheduling --gateway ${GATEWAY} --disable-metrics-collection

ℹ️  📂 Setting up script environment...
ℹ️  kubectl can reach to a running Kubernetes cluster.
ℹ️  🔹 Using merged values: /Users/selvik/stuff/dev/llm-d-infra/charts/llm-d-infra/values.yaml
✅ HF_TOKEN validated
✅ Gateway type validated
ℹ️  🏗️ Installing GAIE Kubernetes infrastructure…
✅ 📜 Base CRDs: Installing...
customresourcedefinition.apiextensions.k8s.io/gatewayclasses.gateway.networking.k8s.io created
customresourcedefinition.apiextensions.k8s.io/gateways.gateway.networking.k8s.io created
customresourcedefinition.apiextensions.k8s.io/grpcroutes.gateway.networking.k8s.io created
customresourcedefinition.apiextensions.k8s.io/httproutes.gateway.networking.k8s.io created
customresourcedefinition.apiextensions.k8s.io/referencegrants.gateway.networking.k8s.io created
✅ 🚪 GAIE CRDs: Installing...
customresourcedefinition.apiextensions.k8s.io/inferencemodels.inference.networking.x-k8s.io created
customresourcedefinition.apiextensions.k8s.io/inferencepools.inference.networking.x-k8s.io created
✅ 🎒 Gateway provider 'gke-l7-regional-external-managed': Installing...
✅ GAIE infra applied
ℹ️  📦 Creating namespace llm-d-inference-scheduling...
namespace/llm-d-inference-scheduling created
✅ Namespace ready
ℹ️  🔐 Creating/updating HF token secret...
secret/llm-d-hf-token created
✅ HF token secret `llm-d-hf-token` created with secret stored in key `HF_TOKEN`
ℹ️  Fetching OCP proxy UID...
ℹ️  No OpenShift SCC annotation found; defaulting PROXY_UID=0
"bitnami" has been added to your repositories
ℹ️  🛠️ Building Helm chart dependencies...
Hang tight while we grab the latest from your chart repositories...
...Successfully got an update from the "nvidia" chart repository
...Successfully got an update from the "weaviate" chart repository
...Successfully got an update from the "metrics-server" chart repository
...Successfully got an update from the "kedacore" chart repository
...Successfully got an update from the "vmware-tanzu" chart repository
...Successfully got an update from the "sonarqube" chart repository
...Successfully got an update from the "kuberay" chart repository
...Successfully got an update from the "nvidia-k8s" chart repository
...Successfully got an update from the "langfuse" chart repository
...Successfully got an update from the "cilium" chart repository
...Successfully got an update from the "deliveryhero" chart repository
...Successfully got an update from the "yugabytedb" chart repository
...Successfully got an update from the "eks" chart repository
...Successfully got an update from the "haproxytech" chart repository
...Successfully got an update from the "stable" chart repository
...Successfully got an update from the "prometheus-community" chart repository
...Successfully got an update from the "fairwinds-stable" chart repository
...Successfully got an update from the "bitnami" chart repository
Update Complete. ⎈Happy Helming!⎈
Saving 1 charts
Downloading common from repo https://charts.bitnami.com/bitnami
Deleting outdated charts
✅ Dependencies built
ℹ️  Metrics collection disabled by user request.
ℹ️  🚚 Deploying llm-d-infra chart with /Users/selvik/stuff/dev/llm-d-infra/charts/llm-d-infra/values.yaml...
Release "infra-inference-scheduling" does not exist. Installing it now.
NAME: infra-inference-scheduling
LAST DEPLOYED: Fri Aug  1 15:37:01 2025
NAMESPACE: llm-d-inference-scheduling
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
Thank you for installing llm-d-infra.

Your release is named `infra-inference-scheduling`.

To learn more about the release, try:

```bash
$ helm status infra-inference-scheduling
$ helm get all infra-inference-scheduling
```
✅ infra-inference-scheduling deployed
✅ 🎉 Installation complete.
```

## Install gateway API


Deploying Gatways: https://cloud.google.com/kubernetes-engine/docs/how-to/deploying-gateways

gcloud components update


Prereqs before we enable gateways: https://cloud.google.com/kubernetes-engine/docs/how-to/deploying-gateways#requirements

Enable Gateway API:

gcloud container clusters update CLUSTER_NAME \
    --location=CLUSTER_LOCATION\
    --gateway-api=standard


Confirm that gateway classes are available:

kubectl get gatewayclass

## install gatway class


 % kubectl get gatewayclass
No resources found
selvik@Selvis-MacBook-Pro llmd % vi install.md           
selvik@Selvis-MacBook-Pro llmd % kubectl get gatewayclass
NAME                                       CONTROLLER                                   ACCEPTED   AGE
gke-l7-global-external-managed             networking.gke.io/gateway                    True       2m24s
gke-l7-gxlb                                networking.gke.io/gateway                    True       2m24s
gke-l7-regional-external-managed           networking.gke.io/gateway                    True       2m24s
gke-l7-rilb                                networking.gke.io/gateway                    True       2m24s
gke-passthrough-lb-external-managed        networking.gke.io/persistent-ip-controller   True       9m4s
gke-passthrough-lb-internal-managed        networking.gke.io/persistent-ip-controller   True       9m4s
gke-persistent-regional-external-managed   networking.gke.io/persistent-ip-controller   True       9m5s
gke-persistent-regional-internal-managed   networking.gke.io/persistent-ip-controller   True       9m5s


 % helm list -n ${NAMESPACE} 
NAME                      	NAMESPACE                 	REVISION	UPDATED                             	STATUS  	CHART                   	APP VERSION
gaie-inference-scheduling 	llm-d-inference-scheduling	1       	2025-08-01 16:41:24.545622 -0700 PDT	deployed	inferencepool-v0.5.1    	v0.5.1     
infra-inference-scheduling	llm-d-inference-scheduling	1       	2025-08-01 15:37:01.711924 -0700 PDT	deployed	llm-d-infra-v1.1.1      	v0.2.0     
ms-inference-scheduling   	llm-d-inference-scheduling	1       	2025-08-01 16:41:31.218145 -0700 PDT	deployed	llm-d-modelservice-0.2.0	v0.2.0     

## Enable network services API

"message": "Gateway: Invalid : error cause: gceSync: NetworkServices API is not enabled, but lb_traffic_extensions, lb_route_extensions, or service_lb_policies are present in the load balancer",

Solution:
Enable the Network Services API: You need to enable the Network Services API for your Google Cloud project.
Go to the APIs & Services section in the Google Cloud Console.
Click "Enable APIs and Services" if it isn't already open.
Search for "Network Services API".
Select the API and click "ENABLE".



## next eror

{
                        "lastTransitionTime": "2025-08-02T00:12:39Z",
                        "message": "error cause: gceSync: generic::invalid_argument: Insert: Invalid value for field 'resource.target': 'regions/us-central1/targetHttpProxies/gkegw1-igwu-llm-d-inferenc-infra-inference-schedul-7fdntinn48ok'. An active proxy-only subnetwork is required in the same region and VPC as the forwarding rule.",
                        "observedGeneration": 1,
                        "reason": "Pending",
                        "status": "False",
                        "type": "Programmed"
