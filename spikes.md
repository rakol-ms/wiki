# 1. Understand how Flux can be configured and mapped to K8s 

### a: Manifests@git branch mapped to K8s cluster/namespace 

Configure the K8s cluster and namespace to look for changes to manifest file in a dev branch.  

Open questions to answer: 

What are the configuration elements to setup an on-prem k8s cluster with a git repo? 

Can the configuration be: Git branch/folder <-> cluster, git branch/folder <-> namespace?  

If not, how can support for that gap added? 

Can a wildcard be used to specify branch name during configuration? Eg. dev-* 

How do we setup multiple namespaces to listen on changes to a specific branch? 

### 1.b: Manifests@tag mapped to K8s cluster/namespace 

Configure the K8s cluster/namespace to look for changes to manifest file with a specific git tag eg. dev-foo 

Open questions to answer: 

can commit tags be used to configure?  

If not, how can support for that gap added? 

 

### 1.c: Helm charts@git branch mapped to K8s cluster/namespace 

Configure the K8s cluster and namespace to look for changes to helm charts package in a dev branch.  

Open questions to answer: 

Can helm 2 and helm 3 charts both be supported for GitOps scenario via Azure Arc provided tools? 

If not, what is required to support? 

# 2. Understand how deployment strategies can be configured in a GitOps scenario 

### 2.a: manifest update rolling out to K8s clusters/namespaces 

Configure 2 or more clusters to selectively (canary, blue green) apply changes when the manifest file is updated in git repo 

Open questions to answer: 

how can Flagger be configured in conjunction with flux to enable deployment strategies? 

can flagger configuration be mapped in an Action (such as the Action described GitOps based K8s deployments) ? 

how does it work (or not work) with Service Mesh integration ? 

### 2.b: helm chart update rolling out to K8s clusters/namespaces 

Configure 2 or more clusters to selectively (canary, blue-green) apply changes when the helm chart is updated in the git repo 

 Open questions to answer: 

same questions as in 2.a. 
