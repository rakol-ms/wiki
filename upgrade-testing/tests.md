## Gates frequency change         		
Define a gate in pre deployment conditions for a release and test the evaluation conditions for the gate after ugprading the server. User should be able to change the timeout beyond 24 hours. This change should also mandate the user to change the reevaluation time of gates to minimum of 30 minutes for any timeouts beyond 24 hours		


## Service Endpoints (Service Fabric)


- Create local Service Fabric Cluster
	
- Download zip from https://go.microsoft.com/fwlink/?LinkId=730690
		
		
		
- Run script .\CreateServiceFabricCluster.ps1 -ClusterConfigFilePath .\ClusterConfig.Unsecure.DevCluster.json -AcceptEULA
		
	
	


- From inside the folder


	
	
- Create Service Fabric Service Connection in 2019/2018 server

	

		
		
- Use option  "Windows security using gMSA"
		
		
		
- Uncheck "Use Windows Security"
		
		
		
- Leave cluster SPN option empty
		
		
		
- Cluster endpoint is http://localhost:19000
		
	
	
	
	
- Use "Service Fabric Deploy" task to deploy to Service Fabric
	
	
	
- Upgrade to 2020 Server
	
	
	
- See if connection works well
	
	
	
- See if "Unsecured" option is enabled in Service Connection in 2020 server


## Service Endpoint
Sharing Service Connection for upgrade testing  [Web View](https://microsoft.sharepoint.com/teams/ReleaseManagement/RM/_layouts/15/Doc.aspx?sourcedoc=%7B8ce2d97c-c8f4-4fb3-bb9f-eb4d99d32dde%7D&action=edit&wd=target%28Features%20test%20scenarios.one%7C8770f380-03cd-409b-a240-9b0d4457cd40%2FSharing%20Service%20Connection%20for%20upgrade%20testing%7Cbe25fb5b-9c5f-403f-9231-eaf041709776%2F%29&wdorigin=703&wdpreservelink=1)
