# Telemetry



To establish communication with the sensor, collect and transform the data, and summarize it into a portal.

**AWS IoT Components**

- Identity Service

  *To provide a secure communication channel for the devices to communicate with each other and other services*

  *Authentication offered by the following options:*

  - *MQTT over TLS v1.2*
  - *SigV4 over HTTP or MQTT over Websockets*
  - *custom authentication tokens*

- Device Gateway

  *To enable devices to securely and efficiently communicate with AWS IoT Core (one-to-one, one-to-many).*

- Message Broker

  *To process and route data from your devices into IoT core.*

  *It is scalable, has low latency and provides reliable message routing. It also uses a Publish and Subscribe model to decouple devices and applications.*

- Rules Engine

  *To take action with the data that match a rule.*

- Device Shadow

  *To store and retrieve current-state information for a thing - meaning a device, an application, and so on.*

- Registry

  *a database of devices that allows you to search for things based on attributes.*

**Thing**

AWS IoT thing is a representation of a physical device or logical entity, like a lightbulb or sensor. It can have attributes, or name-value pairs, that are used to store information about the thing.

```json
{
	"version": 3,
	"thingName": "truckSensor01",
	"defaultClientId": "truckSensor01",
	"thingTypeName": "sensor_DoorAndPower",
	"attributes": {
		"deviceID": "T001",
		"powerRequirements": "12V"
	}
}
```

**Thing Type**

AWS IoT things can be associated to a thing type(not must be) but can be associated to only one thing type at a time.

A thing type is a method to organize things into logical categories; such as, lightbulbs, thermostats, and motion sensors. All things that are associated with the same thing type share a set of common attributes, which can be searchable. 

- *After thing types are created, you cannot change their name.*
- *After a thing type is deprecated, you can decide whether to keep it for historical purposes or delete it.*

Thing type names must be unique within your account.

```json
{
	"thingTypes": [
        {
            "thingTypeName": "sensor_DoorAndTemp"
            "thingTypeProperties": {
                "searchableAttributes": [
                    "deviceID",
                    "powerRequirements"
                ],
                "thingTypeDescription": "sensor for freezer trucks"
            },
            "thingTypeMetadata": {
                "deprecated": false,
                "creationDate": 1468423800950
            }
        }
	]
}
```

Things with a thing type can have up to 50 attributes.

Things without a thing type can have up to 3 attributes.

**Thing Group**

AWS IoT thing groups are another organizing tool that allow you to manage several things at once. Thing groups can have parent-child relationships that allow policies to be attached to a parent group and inherited by their children.

```json
{
	"thingGroupName": "truckSensors",
	"thingGroupArn": "arn:aws:iot:us-west-2:123456789012:thinggroup/truckSensors",
	"thingGroupId": "12345678abcdefgh12345678ijklmnop12345678",
	"version": 1,
	"thingGroupMetadata": {
		"creationDate": 1478299948.882
		"parentGroupName": "truckEquipment"
		"rootToParentThingGroups": [
			{
				"groupArn": "arn:aws:iot:us-west-2:123456789012:thinggroup/truckEquipment"
			},
		]
	},
}
```



To configure and test the device, perform the following steps.

Step1: Unzip the connection kit on the device

```shell
unzip connect_device_package.zip
```

Step2: Add execution permissions

```shell
chmod +x start.sh
```

Step3: Run the start script. Messages from your thing will appear below.

```shell
./start.sh
Waiting for messages from your device
```

**Security**

- Device Authentication

  *AWS IoT Core uses <u>certificates (X.509)</u> to grant access from the device to the core components.*

- Device Authorization (.JSON file)

  - IoT policies -> topics
  - IAM roles -> services

  ```json
  {
      "Version": "2018-10-17",
      "Statement": [{
          "Effect":"Allow",
          "Action":["iot:Publish"],
          "Resource":["arn:aws:iot:us-east-1:123456789012:topic/truck/freezer"]
      },
      {
          "Effect":"Allow",
          "Action":["iot:Connect"],
          "Resource":["*"]
          }]
  }
  ```

*All communication is encrypted with Transport Layer Security, or TLS, v1.2.*

**Message Queuing Telemetry Transport**

A lightweight, open standard messaging protocol which is used within topic publications between authorized identities (devices and applications) and AWS IoT Core.

```json
{
    "serialNumber": "ABCDEFG12345",
    "clickType": "SINGLE",
    "batteryVoltage": "2000mV"
}
```

Device -> Publish a message -> topic <- Subscribe <- "interested" parties

**Registry**

A catalog of static metadata(thing state, parameters, device certificates) and attributes about the devices.

Benefits:

- an optional service
- fully managed and scalable to over a billion devices
- a repository of device certificates

**Rules Engine**

one of the primary methods of filtering and directing communication from AWS IoT Core to other services.

- Helps IoT Core to interact with AWS services
- Monitor the MQTT topic stream
- Rules: Use an SQL-like format to filter MQTT messages

*Be sure to grant the rules engine the appropriate IAM role and permissions so that it can perform any required actions.*

**Analytics** ( +Amazon QuickSight)

->CHANNELS: IoT data 

->PIPELINES: Filter, process, transfer and enrich data(device-specific) 

->DATA STORES: Store raw data and processed data 

->DATASETS: Perform ad hoc queries(built-in SQL) 

->VISUALIZATION: sophisticated IoT analytics and machine learning inference