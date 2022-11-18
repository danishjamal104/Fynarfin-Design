# High Level Design For Approval Module

The main aim of approval module is to approve the entire batch by program manager and admin.

### Conventions
1. The batch will be considered as approved  if any of the program manager and admin approves the batch.
2. The batch can be approved only through the authenticated channel, hence the program manager and admin need to be authenticated to perform this task.
3. This doc assumes that the approver is already authenticated.

---
<img width="382" alt="Screenshot 2022-11-09 at 5 34 30 PM" src="https://user-images.githubusercontent.com/31315800/200826137-1d5f1da6-5369-41b8-8c81-6f1ef06373e4.png">

---

### Approve Batch
The approval by program manager or admin will be considered as a separate event, which is done asynchronously. The gateway with the `pentagon` sign make sure that the batch processing moves forward if any of the(program manager and admin) approves the batch or it gets timeout.
1. The message task(circle with message icon)  named as `Approval By Program Manager` handles the approval by program manager.
2. The message task(circle with message icon) named as `Approval By Admin` handles the approval by admin.
3. The timer task(circle with clock) named as `Approval Timeout` handles the timeout for the approval.

### How to approve a batch?
The authenticated(//todo) program manager and admin can approve the batch by calling an approval API.

### Approval API Spec
This API is only responsible for updating the batch approval status. 
```text
Endpoint: /batchtransactions/{batchId}
Method: PATCH
Header:
{
  "X-CorrelationID": "string" // header parameter to uniquely identify the request. Must be supplied as a UUID.
}
Request Body:
{
    "status": "approved"
}
```
