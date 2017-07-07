
### https://developer.zendesk.com/blog/from-100-requests-to-1-introducing-our-new-bulk-and-batch-apis
> The APIs create an asynchronous background job to do the work, and return a job ID. You can then use the Job Statuses API to check for completion of the job later.

- Request
```
curl https://{subdomain}.zendesk.com/api/v2/organizations/update_many.json?external_ids=external_1,external_2 \  
-H "Content-Type: application/json" -X PUT\  
-v -u {email_address}:{password} \  
-d '{"organization": {"notes": "Something interesting"\}\}'
```
- Response
```
{
  "job_status":{
     "id":"7ba76910ca9f0132378a6cf4bbcdedd1",
     "url":"https://{subdomain}.zendesk.com/api/v2/job_statuses/7ba76910ca9f0132378a6cf4bbcdedd1.json",
     "total":null,
     "progress":null,
     "status":"queued",
     "message":null,
     "results":null
  }
}
```


### https://developers.facebook.com/docs/graph-api/making-multiple-requests
> Batching allows you to pass instructions for several operations in a single HTTP request. You can also specify dependencies between related operations (described in a section below). Facebook will process each of your independent operations in parallel and will process your dependent operations sequentially. Once all operations have been completed, a consolidated response will be passed back to you and the HTTP connection will be closed.


### https://cloud.google.com/storage/docs/json_api/v1/how-tos/batch
> Each HTTP connection that your client makes results in a certain amount of overhead. The Google Cloud Storage JSON API supports batching, to allow your client to put several API calls into a single HTTP request.

### https://developer.mailchimp.com/documentation/mailchimp/guides/how-to-use-batch-operations/
> With batch operations in the MailChimp API, you can complete more than one operation in just one call. Batch operations run in the background on MailChimp’s servers. Clients can poll the API periodically to check batch status until they’re completed, or you can configure a batch webhook to be automatically alerted when a batch process completes.

```
{
  "operations": [
  {
    "method": "GET", # The http verb for the operation
    "path": "/campaigns", # The relative path of the operation
    "operation_id": "my-id", # A string you provide that identifies the operation
    "params": {...}, # Any URL params, only used for GET
    "body": "{...}" # The JSON payload for PUT, POST, or PATCH
  }, …
 ]
}
```


### https://www.meetup.com/meetup_api/docs/batch/
> You may supply a limited number of API requests, typically 1 to 4, in one batch request using the required "requests" parameter. Each of these individual batched requests will be tallied separately the same way they would when making individual requests

```
[{
  "path":"/2/member/self"
},{
  "path": "/2/events",
  "params": {
    "member_id":"self",
    "rsvp": "yes",
    "only":"name,time"
  }
}]
```

### http://docs.aws.amazon.com/AWSECommerceService/latest/DG/BatchandMultipleOperationRequests.html
> Product Advertising API enables you to improve performance by submitting more than one request at the same time. There are two ways to do this:

- Batch request
- Multiple ItemIds

### https://dev.office.com/sharepoint/docs/sp-add-ins/make-batch-requests-with-the-rest-apis
> Learn how to use the  $batch query option with the REST/OData APIs.

- http://www.odata.org/documentation/odata-version-3-0/batch-processing/
- http://www.andrewconnell.com/blog/part-1-sharepoint-rest-api-batching-understanding-batching-requests

### https://developer.salesforce.com/docs/atlas.en-us.api_rest.meta/api_rest/resources_composite_batch.htm
> Executes up to 25 subrequests in a single request (The requests in a batch are called subrequests). The response bodies and HTTP statuses of the subrequests in the batch are returned in a single response body. Each subrequest counts against rate limits.

```
{
"batchRequests" : [
    {
    "method" : "PATCH",
    "url" : "v34.0/sobjects/account/001D000000K0fXOIAZ",
    "richInput" : {"Name" : "NewName"}
    },{
    "method" : "GET",
    "url" : "v34.0/sobjects/account/001D000000K0fXOIAZ?fields=Name,BillingPostalCode"
    }]
}
```


### https://www.fullcontact.com/developer/docs/batch/
> The Batch Process endpoint allows you to batch several API requests into a single request to improve performance by eliminating excess network overhead. This endpoint will proxy requests (up to 20 per batch) on your behalf to any other API endpoint which accepts GET requests.

- Request
```
POST /v2/batch.json?apiKey=[your API key] HTTP/1.1
Content-type: application/json
Host: api.fullcontact.com
Content-Length: 236
Connection: Keep-Alive
{
    "requests" : [
        "https://api.fullcontact.com/v2/name/normalizer.json?q=dan+lynn",
        "https://api.fullcontact.com/v2/name/normalizer.json?q=kyle+hansen",
        "https://api.fullcontact.com/v2/person.json?email=bart@fullcontact.com"
    ]
}
```
- Response
```
{
  "status": 200,
  "responses": {
    "https://api.fullcontact.com/v2/name/normalizer.json?q=dan+lynn": {
      "region": "USA",
      "nameDetails": {
        ...
      },
      "status": 200,
      "likelihood": 0.974
    },
    "https://api.fullcontact.com/v2/name/normalizer.json?q=kyle+hansen": {
      "region": "USA",
      "nameDetails": {
        ...
      },
      "status": 200,
      "likelihood": 1
    },
    ...
  }
}
```


## References
- http://apievangelist.com/2017/05/17/studying-how-providers-are-supporting-batch-api-requests/
