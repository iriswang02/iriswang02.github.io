# Fleet Management



Main pillars:

Onboard -> Organize -> Monitor -> Update

**Onboard**

AWS IoT Device Management allows you to onboard devices at scale through the command line interface(CLI).

- customer authorization
- bulk provisioning

**Organize**

Using thing groups allow to set policies for devices at an organizational level, instead of at the device level.

AWS IoT Device Management also provides indexing, which can make searching the AWS IoT Device Registry more efficient and effective. 

- thing groups, group policies
- search and indexing services

**Monitor**

Monitoring device activities informs you about changes made to your devices and who made them. 

- metrics, logging capabilities
- event communication

**Update**

By using AWS IoT Jobs, you can organize and schedule device updates in a controlled manner.

- device jobs that can be sent to individual things and groups of things

#### Preparing for Fleet Management

When preparing for fleet management, it's important to track device activity through registry events, provision your devices through the API, and organize your fleet by configuring thing groups.

If you enable device notifications for registry events, AWS will publish messages over MQTT with a JSON payload whenever a thing, thing type, or thing group is 

- created, (`$aws/things/<thingName>/shadow/create`)
- updated, (`$aws/things/<thingName>/shadow/update`)
- or deleted. (`$aws/things/<thingName>/shadow/delete`)

```json
{
    "eventType": "thingEvent",
    "eventId": "fsdfw123-423f-23r2-23r2-b23rsfij43f4",
    "timestamp": 1234567890123,
    "operation": "CREATED|UPDATED|DELETED",
    "accountId": "123456789012",
    "thidngId": "b604f69c-aa9a-3e4r-4e54-crjr489je455",
    "thingName": "MyThing",
    "versionNumber": 1,
    "thingTypeName": null,
    "attributes": {
        "attribute3": "value3",
        "attribute1": "value1",
        "attribute2": "value2"
    }
}
```

#### AWS IoT Thing Groups

-> By configuring your devices into thing groups, you can organize your devices and easily search the fleet.

-> Using thing groups to aid in searching because attributes can be added, updated, or removed at the group level.

-> Configure logging options for all the things within a group. These logs keep you informed of any changes that occur within the group, which is useful for debugging.

A thing can be a member of up to 10 groups.

Using thing groups, we're able to organize our things into logical hierarchies. This enables child things inherit all attributes of parent things. If establish parent-child relationships by having groups inside of groups, it allows child groups to inherit permissions and policies from their parents. But each thing within the group will still need to have their certificate.

#### Device Provisioning

Device provisioning allow you to register your connected devices individually or in bulk. When you provision a device, you create and register the following:

- A certificate
- A policy, which is attached to the certificate
- A unique identifier for your thing
- And attributes associated with the thing, which might include associating the thing to a thing type and/or to one or more thing groups.

**Provision in advance**

- API calls (must have an X.509 certificate that is registered with AWS IoT and an active IoT policy.)

  - First, the IoT device sends its thing name to the Amazon API gateway.
  - When the gateway receives the call, it triggers an AWS Lambda function.
  - Then, a query is sent to the DynamoDB database to check if the thing can be provisioned.
  - Amazon DynamoDB sends either of these responses to the AWS Lambda function:
    - If the device is not allowed, it sends an error message, or
    - If the device is allowed, then the appropriate region is determined, the device is provisioned in the AWS IoT, and the DynamoDB is updated.
  - Next, the Lambda function sends the appropriate response to the Amazon API gateway.
  - If the device was successfully provisioned, endpoint, key, and certificate are sent to the device.
  - Finally, the newly provisioned device connects to the selected region.

  The output for some commands is stored in files in /tmp because values from it are required for further steps in the provisioning chain.

  Once IoT Events are enabled, you can see the device registry's messages by subscribing to the related topic hierarchy.

- single device provisioning

- bulk device provisioning (if you have a large number of known devices whose desired characteristics you can assemble into a list)

**Provisioning Template**(.JSON file)

To provision in advance (single or bulk), provisioning template is needed. It uses parameters to describe the resources your device must use to interact with AWS IoT.

```json
"Parameters": {
    "ThingName": {"Type": "String"},
    "SerialNumber": {"Type": "String"},
    "Location": {"Type": "String",
                "Default": "WA"},
    "CSR": {"Type": "String"}
}
```

The resources section declares the resources that are required for device to communicate with AWS IoT.

```json
"Resources":{
    "thing": {
        "Type": "AWS::IoT::Thing",
        "Properties":{
            "ThingName": {"Ref": "ThingName"},
            "AttributePayload":{
                "version": "v1",
                "serialNumber": {"Ref": "SerialNumber"}
            },
            "ThingTypeName": "lightBulb-versionA",
            "ThingGroups": ["v1-lightbulbs",{"Ref": "Location"}]
        }
    },
    "certificate": {"Type": "AWS::IoT::Certificate",
                   "Properties":{"CertificateSigningRequest":{"Ref": "CSR"},
                                "Status": "ACTIVE"}}
}
```

For single device provision, you define a single line so that you can register your thing.

For bulk device provision, you will create a list and save it to an Amazon S3 bucket. Then you need to initiate the StartThingRegistrationTasks API to provision multiple devices that provisions the things in the background. Once initiated, you receive a Task ID that you can use to query the service for the task status.

Each account can run only one bulk provisioning operation task at a time.

Key provisioning related APIs:

- StartThingRegistrationTasks 
- ListThingRegistrationTasks
- DescribeThingRegistrationTasks 
- StopThingRegistrationTasks
- ListThingRegistrationTaskReports

**Provision on demand**(if you dont have information about the large number of devices that you can assemble into a bulk provisioning list)

- Just-in-Time Provisioning

  1. Register your own certificate authority(CA) with AWS IoT
  2. Enable automatic registration within AWS IoT
  3. Associate the provisioning template to your CA
  4. Generate your own Public Key Infrastructure keys and certificates, and then copy them to your devices.

  When your device connects to AWS IoT for the first time, AWS IoT automatically calls the RegisterThing API, its certificate is registered, and a notification is published to an internal topic ($aws/events/certificates/registered/certificateID)

- Just-in-Time Registration

  By using the AWS IoT Rules engine to monitor this topic, the registration event can trigger an AWS Lambda function which is used to provision the thing differently depending on what kind of thing it is.

  With Just-In-Time Registration, when a device connects for the first time, its certificate initially registers with a PENDING_ACTIVATION status. IoT Core then publishes a notification. An IoT Core Rule, which monitors this topic, triggers a Lambda function to evaluate the device template and provision it according to your instructions. By the end of this process, the Lambda function updates the certificate status to active.

#### Managing the Fleet

AWS IoT Jobs lets you send remote actions to your devices that are connected to AWS IoT.

A Job is remote operation that is sent to and executed on one or more devices that are connected to AWS IoT. An agent on each device is needed to execute jobs. To execute a job, the agent processes a job document.

Before creating a job, you need to create a job document, which is a JSON file that defines actions that the device should take locally. 

Jobs can be created through the AWS Console, CLI, and SDK.

1. Specify a list of targets that are the devices that should perform the operations. The targets can be things or thing groups or both. AWS IoT sends a message to each target to inform it that a job is available.
2. Create job instance on target device
3. Target device downloads the job document
4. Target Device performs operations specified in document
5. Target Device reports progress to AWS IoT.

**Job documents**

```json
{
    "operations": {
        "reboot": "safe-mode",
        "configurations": {
            "log": "persist",
            "download":{
                "target": "${aws:iot:s3-presigned-url:https://s3.amazonaws.com/bucket/key}",
                "patch": "critical"
            },
            "restart": "blemodule"
        }
    }
}
```

To allow a device secure, time-limited access to data beyond that included in the job document itself, you can use presigned Amazon S3 URLs. Presigned URLs could be used to obtain firmware, software, etc. A roleArn must be included in the pre-signed URL's configuration to allow the download of files from S3. You can also require the services of a roleARN. This is the ARN of an IAM role that grants permission to download files from the S3 bucket

**Job agent**

The job-agent runs permanently on the device or fleet of devices.

1. Subscribed to topic `$aws/<...>/get/accepted`
2. Publish to get a job `$aws/<...>/get` (get the first job from a list of queued jobs)
3. AWS IoT publishes the answer to the request above `$aws/<...>/jobs/<job-id>/get/accepted`
4. The answer contains the job document. The job-agent parses the job document and acts after the instructions in the job document.
   - The agent starts the job execution
   - It reports the status IN_PROGRESS to the topic `$aws/<...>/jobs/<job-id>/update` and back to AWS IoT
5. The job-agent then process the job document and completes the job
6. The job agent reports either the status SUCCEEDED or FAILED to the topic `$aws/<...>/jobs/<job-id>/update` and back to AWS IoT.

#### Monitoring Devices

- updates
- joining groups
- security policies (you can not only know when changes are made but also who made those changes)

You need to use the UpdateEventConfigurations API to control what kinds of job events you receive.

**Fleet indexing service**

Fleet indexing is a managed service that enables you to index and search your registry and shadow data in the cloud. AWS_Things is the index created for your things and AWS_ThingGroups is the index created for your thing groups.

Once Fleet indexing service is active, you can use a simple query language to search across this data.

**Best Practices**

Things:

- Utilizing granular device IDs and associate them with the collected and aggregated device data. This provides overall flexibility and a better ability to scale horizontally.
- Applying the least privilege principle to policies and
- Associating your thing to a thing type

Bulk provisioning

- Rolling out updates gradually on your fleet
- Tracking hardware versions and keep backwards compatibility as much as possible regarding hardware resources, sometimes you may have older hardware on the field
- Finding a way to always update the devices in future as device update is the holy grail and the mechanism you will rely on to fix bugs

Jobs

- Job agent: When implementing a job agent, create a core implementation that does not need an update. Extended functionality of a job agent could be implemented as modules to minimize the risk the a complete update of the agent leads to a broke installation.
- Monitor job execution: AWS IoT Core publishes information with regards to job and job execution to reserved topics under $aws/events/#. Monitor the topics $aws/events/jobExecution/# and $aws/events/job/#. By using topic rules, failed jobs could be handled automatically.
- Bootstrapping: A combination of registry events, topic rules, Lambda, and jobs could be used to bootstrap devices.

Securing Devices

- Provide a unique certificate and unique private key for each device to enable fine-grained management including certificate revocation. Devices must also support rotating and replacing certificates in order to ensure smooth operation as certificates expire.
- Ensure the ClientID and Thing Name match on device connections
- IoT policies should follow a strategy of least privilege for the permissions that it grants.
