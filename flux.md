#### Flux: configuring git branch vs tag to trigger a K8s deployment

Things covered:  

1. How to setup Azure Arc?

  -  Follow the tutorial [here](https://github.com/Azure/ClusterConfigurationAgent/blob/master/README.md)
     - It's basically a helm chart deployment 
     - Then you can use commands like ```az rest --method PUT --uri ${CONFIG_URI} --uri-parameters api-version=2019-11-01-preview --body @./examples/read-only-repo.json``` inorder to apply configurations on the connected cluster. 

1.a. How does the apply of the config work? 

1.b. What does the helm deployment do?

  - Find the resources being created [here](https://github.com/Azure/ClusterConfigurationAgent/tree/master/azure-arc-k8sagents/charts/setupChart/templates)
     - Connected Cluster Agent
        - It fetches the cluster info
        - Creates resource group if doesn't exist
        - Creates a new resource on azure side of type connectedCluster if it doesn't exist
     
     - Cluster Configuration Operator
     - Configuration Agent

    
2. What can flux do? 


#### Deploying Helm 3 charts with Flux 

#### Deployment strategies using Flux and Flagger

#### Stabilize deployment status when using GitOps

#### Annotating deployment when using Helm, Kubectl, Flux

#### Communicating with Azure Relay when using Azure Arc

#### Adding private clusters to environment

#### Delta between 2 deployments

#### Flux deployment based on container registry trigger

#### Rollback mechanism when using Flux
