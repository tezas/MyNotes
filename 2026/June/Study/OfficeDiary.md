
## Contribution to the team level operations
### Situation:
1. There was a system where one writer worker listens to the events and creates mapping using some business logic between the event and the data residing in the system.
2. Another worker is listening the events from upstream and updating the status in the same mapping generated in step 1.
3. These 2 lambdas are getting deployed in same pipeline as they serve a common objective.
   
### Task
1. A developer was writing the integration tests in the pipeline and the question raised if 2 different tests for the 2 workers should be enough or a single
2. test for the complete lifecycle involving 2 different workers is needed.

### Action
4. Mentioned that if there is a COE written because of an issue in pipeline because of the enhancement introduced in 1 st worker which can cause adverse impacts on 2nd worker like introducing new data type while writing the data then we would need to revisit and see how could have we avoided the issue and then conclusion would be to write test cases which ensures the contract between the 2 workers.


## Launch Bulk Import/Export

1. We pushed all the changes to production.
2. For APM Integration, we needed to perform backfill for existing connectivity. At the time of launch it was already performed but there was a timeline issue as between completion of backfill and launch, days were spent in which some sites created new data which needed another iteration of backfill. This came as a surprise in last minute and pushed the launch needed to another day.
3. On another thread, while performing the App release which had some final changes needed for new features, we deployed the changes to production unintentionally for [pilot sites for ISO V2]. This created some churn which could have been avoided. I am planning to write a retrospective document on the App release to ensure this doesn't repeat.
4. All features were hidden behind feature gate at UI and backend. We noticed while deploying the changes in production it took 30 minutes for each profile and it was deploying in sequential. [How did we realise it?: When we saw that profile for frontend is enabled we started testing which was showing the new features on UI but backend feature flag was not enabled because of which frontend was throwing error for the meantime.] There fore we took an action item to segregate the frontend and backend separately and then deploy as in a single environment deployment.
