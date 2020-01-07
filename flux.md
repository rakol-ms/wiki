## 1. How to setup Azure Arc?

  -  Follow the tutorial [here](https://github.com/Azure/ClusterConfigurationAgent/blob/master/README.md)
     - It's basically a helm chart deployment 
     - Then you can use commands like ```az rest --method PUT --uri ${CONFIG_URI} --uri-parameters api-version=2019-11-01-preview --body @./examples/read-only-repo.json``` inorder to apply configurations on the connected cluster. 

#### 1.a. How does the apply of the config work? 

#### 1.b. What does the helm deployment do?

  - Find the resources being created [here](https://github.com/Azure/ClusterConfigurationAgent/tree/master/azure-arc-k8sagents/charts/setupChart/templates)
     - Connected Cluster Agent
        - It fetches the cluster info
        - Creates resource group if doesn't exist
        - Creates a new resource on azure side of type connectedCluster if it doesn't exist
        - This new type of resource can be browsed from the Azure portal for managing.
     
     - Configuration Agent
        -  Internally creates a cluster configuration operator
           - Current offerings: Flux, Helm
        - The configuration operator translates configs like this: 
          ```json
            {
              "properties": {
                "repositoryUrl": "git://github.com/ds-ms/app.git",
                "operatorNamespace": "sample-app",
                "operatorInstanceName": "sample-go",
                "operatorType": "flux",
                "operatorScope": "cluster",
                "operatorParams": "--git-email desattir@microsoft.com --git-readonly --git-path=manifests --git-user=ds-ms"
              }
            }
          ``` 
          to desired configuration setups.
          In the above example it would create a flux configuration to listen to a particular github repo.

    
2. What can flux do? 
