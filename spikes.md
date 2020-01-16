# 1. Understand how Flux can be configured and mapped to K8s 

### a: Manifests@git branch mapped to K8s cluster/namespace 

Open questions to answer: 

#### 1. What are the configuration elements to setup an on-prem k8s cluster with a git repo? 

How to set up flux on your cluster? 

0. In order to install flux on your on-premise cluster; you first need `fluxctl` which can be installed from [here](https://docs.fluxcd.io/en/latest/references/fluxctl.html)
1. Create a namespace called `flux`
2. Then using fluxctl set up a configuration like this:
  ```sh
     export GHUSER="YOURUSER"
     export REPONAME="flux-get-started"
     fluxctl install \
     --git-user=${GHUSER} \
     --git-email=${GHUSER}@users.noreply.github.com \
     --git-url=git@github.com:${GHUSER}/${REPONAME} \
     --git-path=namespaces,workloads \
     --namespace=flux | kubectl apply -f -
  ```
  and then wait for flux to start.
3. The flux pods on your cluster wouldn't have permissions to write to your repo by default. So, you'll have to get the public key of the flux service and add it to your repo's settings.
  - navigate to `https://github.com/<user>/<reponame>/settings/keys`
  - Get the ssh key by running `fluxctl identity --k8s-fwd-ns flux`
  - Paste that ssh key in the above opened window. (i.e. create a new entry)

4. [Optional]  Flux syncs your repo every 5 minutes. But if you want to sync it explcitly you can run `fluxctl sync --k8s-fwd-ns flux`



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
