
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
