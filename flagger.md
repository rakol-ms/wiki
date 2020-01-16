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
