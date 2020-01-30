## In a GitOps based flow, how can the user track which deployments ? states such as queued, started, in-progress, completed (success/failure) 

    - State changes of resources
    - Logs of the gitops operator (i.e. flux)

## In a normal flow (kubectl), how can GitHub Action deploy to a connected cluster ? Is Azure relay going to come into picture or there will be some other mechanism possible to access the cluster for executing the kubectl/helm release commands. 

As per the discussions we had with the Arc team; they will end up creating a kubeconfig which points to the Azure Relay endpoint which will set up the communication between action runner and the private cluster.


## What level of details can be added to a Deployment? any restrictions? 

Find all the details you can add to a deployment here: https://developer.github.com/v3/repos/deployments/#parameters-1 
When you create a deployment using this rest api, it gives you a response which would contain a deployment ID. You can POST the deployment statuses to that particular ID to acheive full tracability. 

## How can multiple deployment statuses (to multiple environments) be handled when using Deployment strategies? 

Multiple statuses to multiple environemnts can be achieved using the above logic I mentioned. But that would be in a GitOps world.
Now in the combination we used i.e. flagger + flux.. Flagger only listens to the state changes of a resource object and Flux only takes care of updating the resource object. So, flagger doesn't have any context of the deployment environments of GitHub as such.