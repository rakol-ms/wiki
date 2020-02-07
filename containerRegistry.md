### How can flux be configured to listen on updates to container registry/repo and update the K8s cluster/namespace?

Flux listens to the container registry that has been mentioned in the object deployed in the cluster. It deploys and maintains a memcache service which has a cache of the image deployed, and is reacts to tag updates and pulls the image. Therefore updating tags is the way to inform flux to update the image

An alternate way would be to have a docker push task followed by a commit to the cluster repo. We will push and get the new hash of the container image we just deployed. We will update the image hash in the deployment object in the manifest file
```
      image: foo.azurecr.io/myrepo:<hash_that_needs_to_be_updated>
```
Flux will sync and update to the commit, and therefore update the image
