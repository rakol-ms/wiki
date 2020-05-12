Engineering things needed post moda service deploy
---------

### How to setup datadog monitor?
   - [Documentation link](https://github.com/github/observability/tree/master/docs/datadog) (resource metrics)
   
      a. After setting up monitor you get an ID (Get entitlements for your datadog access before getting into all this)
      
      b. export that monitor (will export all details in text)
      
      c. Generate a teams alias nines app (https://github.com/github/ninesapp)
          - You'll need to create a regex for matching the datadog monitor pattern for redirecting to your team
          - You can onboard for slack notifications as well
      
      d. Commit that text file in https://github.com/github/datadog-monitoring
      
      e. The created monitor would be deleted and then an automated immutable monitor would be created using the same spec.
      
      f. You can add new widgets to your dashboards

  - This would cover GLB metrics as well along with the status codes
  - You can onboard app to datadog-stats-d for getting specific app metrics [Future]
      + You can create an app metrics dashboard within datadog + monitors

### How did you onboard to pagerduty(only for Sev1s)?
  - Raise entitlements request for pager-Duty
  - Setup 3 things (Get the URL from okta after entitlements)
      + Create team
      + Create a schedule for primary/secondary DRI
      + Escalation policy

### What kind of metrics did you use in Splunk?
  - Splunk query for processing stdout
  - The incidents would only get triggered by mail
  - In Future move to datadog


### [Architecture](https://github.com/github/observability/blob/master/docs/diagrams/overview/overview.png)

### Future
Onboard to Sentry, looker, Lightstep in future




