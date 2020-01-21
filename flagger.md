# Manifest update rolling out to K8s clusters/namespaces 

Configure 2 or more clusters to selectively (canary, blue green) apply changes when the manifest file is updated in git repo 
- The canary deployment doesn't work across clusters. It is configured/promoted internally. i.e. similar to what our manifest task does.

## How can Flagger be configured in conjunction with flux to enable deployment strategies? 

Testing resources:
 - isito installed on your cluster
 - This sample app: https://github.com/weaveworks/flagger/tree/master/kustomize/podinfo
 - Load testing service: https://github.com/weaveworks/flagger/tree/master/kustomize/tester
 - CRD used for configuring Canary: [canary-crd.yml](resources/canary-crd.yml)
 
Flagger takes app selectors as a parameter to figure the target, something like this:

```yaml
targetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: podinfo
```

If you notice the kustomize manifests, we're only deploying 2 resources; i.e. a deployment and an optional hpa (horizontal pod autoscalar).

Once you apply the canary CRD:

### Initialization 
A few resources get automatically created.
 - Services
    - app
    - app-canary
    - app-primary
- Deployments
   - app-primary
- Virtual Service (istio object if selected)
   - app

The virtual service object defines the routing/loadbalancing in this example. 

### Post intialization
After all the required resources are created, flagger listens to changes to that particular deployment mentioned in the CRD. 

### Flux involvement
So, now let's say you've configured your repo with flux. And every time you make a check-in to your repo, flux updates the deployment object. This internally triggers flagger.

There are 2 deployment objects all the time; i.e. app & app-primary. 

#### Current
States: 
 - app - v1 [0 pods]
 - app-primary - v1 [2 pods]

#### After check in
States:
 - app - v2 [2 pods]
 - app-primary - v1 [2 pods]

And the weights on the VirtualService keep getting manipulated as we progress with the canary deployment.
The logs look like this:

```
"New revision detected! Scaling up app.test"
"Starting canary analysis for app.test"
"Pre-rollout check acceptance-test passed"
"Advance app.test canary weight 10"
"Advance app.test canary weight 20"
"Advance app.test canary weight 30"
"Advance app.test canary weight 40"
"Advance app.test canary weight 50"
"Copying app.test template spec to app-primary.test"
"Routing all traffic to primary"
"Promotion completed! Scaling down podinfo.test"
```

#### After promotion is completed

States:
 - app - v2 [0 pods]
 - app-primary - v2 [2 pods]

------

## Can flagger configuration be mapped in an Action (such as the Action described GitOps based K8s deployments) ? 
I don't completely understand this question. 


## how does it work (or not work) with Service Mesh integration ? 
It works with SMI. During the update process, the weights of the service routing are manipulated. 

This is the default state of the traffic split object:
```yaml
 Route:
      Destination:
        Host:  app-primary
      Weight:  100
      Destination:
        Host:  app-canary
      Weight:  0
```

After a change is detected, the app-canary's weight is increased and app-primary's weight is decreased to distribute the traffic during promotion of v2. 
Looks something like this when a v2 is being promoted:
```yaml
 Route:
      Destination:
        Host:  app-primary
      Weight:  80
      Destination:
        Host:  app-canary
      Weight:  20
```
And eventually it resets to the default state if we are promoting/rejecting the canary. 

