Feature ask:
  When pre-deployment gates are introduced, they have a default max limit of 24 hours on the timeout in evaulation options.
  There is an ask to increase the timeout. But every gate has an execution interval of 5 minutes by default.
  If the user is allowed to evaluate the gate every 5 minutes and have a timeout beyond 24 hours (ask was 3 days) will cause a lot of resource consumption on hosted servers.
  Hence the feature is implemented in such a way that if the user configures his gate evaulation timeout beyond 24 hours, the minimum retry interval is fixed to atleast 30 minutes.
  
  
 Steps to invoke this feature.
  We can see this in two phases.
    1. Before the system is upgraded to the version implementing this feature.
       a. Define a release definition.
       b. Try adding a pre-deployment gate in a stage. It will be the "Tablet" shaped icon infront of a stage.
       c. Observe the min/max values permitted in the evaulation options carousel. You should be able to set a max timeout of 24 hours.
    2. After ugprading the system.
       a. Open the previously defined release definiton.
       b. Edit the pre deployment gate of the RD> 
       c. Expand the evaluation options carousel. Definition before the upgrade should be present and should not have any validation erros on UI.
       d. Try modifying the timeout value. A value beyond 24 hours should be permitted. Also if the re-evaluation time is left default in the previous step (5 minutes) UI should have a validation error with new limits displayed.
       
