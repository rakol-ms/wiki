### Adding annotations from within the task
Annotations can be added to the manifest files and pods from the tasks which specifically deal with deployment to a kubernetes cluster (using a kubernetes service connection)

In each of these cases, we can configure kubectl using the service connection. 
helm install, helm upgrade and kubectl apply all return a manifest as the output when run. Let us call that M1.

We will configure kubectl connection using the service connection data
We can get the pods using
```
kubectl get pods
```

Each pod will provide its owner reference. We can get the current objects being pushed to the cluster using the data in M1. We will filter each of the pods that have an owner reference which is one of these objects. We will add the run details as annotations for each of the pods

```
kubectl annotate pods <pod_name> <annotations> --overwrite
```

If we have the manifest files, we will annotate these files with ```kubectl -f <file_name>```

### Annotate on any usage of kubectl apply, or helm deploy commands inside the agent

We have a process watcher today running inside agents, which tracks the active processes invoked. We can use this to know when one of the commands are invoked. But at the moment, we do not have the capability to access any of the data provided inside a task at the point when we encounter one of these commands. This means that we won't be able to configure kubectl, and we won't be able to invoke ```kubectl annotate```. We currently do not have a framework to securely access user service connection data from the process monitor (a global level process), so we are skipping it for now. We can revisit this later.
