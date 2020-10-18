---
title: "Detecting Runtime Cloud Security Threats"
chapter: false
weight: 52
---

## Detecting Runtime Cloud Security Threats

Let's look at an example of AWS threat detection in action with CloudTrail and the Sysdig Cloud Connector.  To do so we'll create an S3 bucket, and make it public

1. Log into Cloud9 Workspace
2. Create an S3 bucket. S3 bucketnames are globally unique, so you use your initials combined with a timestamp

    ```
    INITIALS=<your initial>
    BUCKETNAME=$INITIALS-$(date +%s)
    aws s3api create-bucket --bucket $BUCKETNAME --acl public-read
    ```

3. Now delete the S3 bucket's encryption.  This should be considered a potential security threat.

    ```
    aws s3api delete-bucket-encryption --bucket $BUCKETNAME
    ```

4. To view details of this event, browse to [CloudTrail](https://console.aws.amazon.com/cloudtrail/home) then 'Event History'

    **Dev Note** Add screenshot here

5. Click the event just created

    If you scroll down you'll see details of the new CloudTrail event that was created.  It looks like this in JSON format:


    ```JSON
    {
        "eventVersion": "1.05",
        "userIdentity": {
            "type": "Root",
            "principalId": "999999999999",
            "arn": "arn:aws:iam::972909301756:root",
            "accountId": "999999999999",
            "accessKeyId": "FE1D489888F66424BBE7",
            "sessionContext": {
                "sessionIssuer": {},
                "webIdFederationData": {},
                "attributes": {
                    "mfaAuthenticated": "true",
                    "creationDate": "2020-06-18T07:52:40Z"
                }
            }
        },
        "eventTime": "2020-06-18T09:05:23Z",
        "eventSource": "iam.amazonaws.com",
        "eventName": "AttachUserPolicy",
        "awsRegion": "us-east-1",
        "sourceIPAddress": "37.132.12.63",
        "userAgent": "console.amazonaws.com",
        "requestParameters": {
            "userName": "admin_test",
            "policyArn": "arn:aws:iam::aws:policy/AdministratorAccess"
        },
        "responseElements": null,
        "requestID": "7fe1d489-550c-527f-6155-70d1522b95f0",
        "eventID": "dd9fe2aa-70d1-4bbf-2ac4-b4766642e73b",
        "eventType": "AwsApiCall",
        "recipientAccountId": "999999999999"
    }
    ```

All CloudTrail events have the following key fields:

- *userIdentity*: The user who sent the request.
- *eventName*: Specifies the type of event.
- *requestParameters*: Contains all of the parameters related to the request.

If a request has an *errorCode* field, it means that it could not be processed because of an error. For example, the requester may not have had permission to perform a change.

In this case, we can see how a policy has just been attached (*AttachUserPolicy*) to a user (*admin_test*) with administrator access (*arn:aws:iam::aws:policy/AdministratorAccess*).

A Falco rule to detect this elevation of privileges would look like this:


```
- rule: Delete bucket encryption
  desc: Detect deleting configuration to use encryption for bucket storage
  condition:
    jevt.value[/eventName]="DeleteBucketEncryption" and not jevt.value[/errorCode] exists
  output:
    A encryption configuration for a bucket has been deleted
    (requesting user=%jevt.value[/userIdentity/arn],
     requesting IP=%jevt.value[/sourceIPAddress],
     AWS region=%jevt.value[/awsRegion],
     bucket=%jevt.value[/requestParameters/bucketName])
  priority: CRITICAL
  tags: [cloud, source=cloudtrail, aws, NIST800_53, NIST800_53_AU8]
  source: k8s_audit
```

This particular rule is already included out-of-the-box in Sysdig Cloud Connector.

The *jevt.value* contains the JSON content of the event, and we are using it in the *condition*. Using the [jsonpath](https://jsonpath.com/) format, we can indicate what parts of the event we want to evaluate.

The *output* will provide context information including the requester *username* and *IP address* - this is what will be sent through all of the enabled notification channels.

As you can see, this is a regular Falco rule. CloudTrail compatibility is achieved by handling its events as JSON objects, and referring to the event information using JSONPath.
