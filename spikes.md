# 1. Understand how Flux can be configured and mapped to K8s 

## Manifests@git branch mapped to K8s cluster/namespace 

### 1. What are the configuration elements to setup an on-prem k8s cluster with a git repo? 

#### How to set up flux on your cluster? 

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

#### What does flux config setup create?

The resources yaml can be found here: [fluxctl-output.yml](resources/fluxctl-output.yml)
To summarize:
  1. [flux] A Deployment object with this container image:  docker.io/fluxcd/flux:1.17.0
  2. [memcached] A deployment with this container image: memcached:1.5.20
  3. A service for communicating with the memcached service
  4. A service account
  5. A cluster role called `flux` 
  6. A cluster role binding for the above created service account



### Can the configuration be: Git branch/folder <-> cluster, git branch/folder <-> namespace?  

The flux configurations do not take a namespace field to figure out where to deploy the manifests paths you've just configured. So, the namespace to which you want to deploy your manifests to, should be a part of the manifests. That would mean: 
  - if you try to set up a same repo config usin different branches to the same cluster; the flux installation would override your existing one. 
  - If you want to set up a repo config with different branch, then each of your branch should have changes to the manifests file declaring the namespace these resources are expected to be in; or you can use `helm` where you can configure your `Release.Namespace` during run time.

##### [TBD] If not, how can support for that gap added? 

### Can a wildcard be used to specify branch name during configuration? Eg. dev-* 
No, flux configurations are static. They can not take wild cards as an attribute to their configuration. If they do, they'd end up falling into the case I mentioned above i.e.
>  if you try to set up a same repo config usin different branches to the same cluster; the flux installation would override your existing one. 

### How do we setup multiple namespaces to listen on changes to a specific branch? 
You *DONT* setup multiple namespaces to listen to changes to a specific. You create a flux configuration which listens to changes to a specific branch; that is in the `flux` namespace. You deploy to mmultiple namespaces by choosing the manifests which deploy across namespaces. 
For instance, during my configuration I've used `--git-paths=service1/manifests,service2/manifests` where service1 creates resources in `namespace1` and service2 creates resources in `namespace2` the flux operator would listen to changes to your repo and apply all the manifests in both the folders. And that would inturn create reosources wherever specified in the manifests. 



## 1.b: Manifests@tag mapped to K8s cluster/namespace 

#### Can commit tags be used to configure?  
No. They don't have support for listening to tags, yet. There is an ongoing issue: https://github.com/fluxcd/flux/issues/2568 

There is a PR in progress: https://github.com/fluxcd/flux/pull/2544 

This PR would introduce a new argument to fluxctl i.e. `--git-tag` & uses this argumment in the flux-operator to listen to changes to that. 

 

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
