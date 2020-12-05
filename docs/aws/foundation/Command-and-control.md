# Command and Control



To control IoT devices and their environment by sending commands and receiving acknowledgements.

#### MQTT

a publish/subscribe messaging transport protocol

- lightweight
- open
- easy implementation
- a small code footprint is required and/or network bandwidth is a premium

Message broker: a publish/subscribe broker service that enables the sending and receiving of messages to and from AWS IoT.

Topics do not need to be set up prior to publishing or subscribing.

- case sensitive
- utilize a hierarchical name space
- each level is separated by a forward slash

a single level wildcard: `home/ac/AAA/temperature`, `home/ac/BBB/temperature`, ...

a multi-level wildcard: `home/ac/#` (covers every topic nested under it)

Reserved topic start with a '$' and are made available by the AWS IoT Core for special operations such as getting or updating a device state. The reserved topic space is namespace managed nu the MQTT broker.

**Device Gateway**

It forms the backbone of communication(low latency & overhead) between connected devices and the cloud capabilities.

#### Device Shadow (.JSON file) (MQTT or HTTP)

The device shadow service maintains a shadow for each device that created in the registry.

- It enables cloud and mobile applications to easily interact with connected and registered devices that may have intermittent access issues.

- It maintains the latest state information for each device.
- It enables you to have a synchronized set of state values between the cloud and device.
- It allows the application to set a device state and have it retrieved when the device reconnects.
- Each device's shadow is uniquely identified by the name of the corresponding Thing.

A device's shadow document contains properties of a connected device:

- Device State (variables and values)

- Metadata (timestamps, in Epoch time, for each attribute in the state section, which enables you to determine when they were updated)

- Timestamp (indicates when the message was transmitted by AWS IoT Core)

- clientToken (A string unique to the device that enables you to associate responses with requests in an MQTT environment)

- Version (refers to the document version. Every time the document is updated, this version number is incremented.)

  *A client can receive an error if it attempts to overwrite a shadow using an older version number. The client is informed it must resync before it can update a device's shadow. A client can decide not to act on a received message if the message has a lower version than the version stored by the client.*

and two states:

- Desired (Applications write to this portion of the document to update the state of a thing)
- Reported (Things write to this portion of the document to report their new state)

**Building Blocks of the Device Shadow:**

1. Device shadow topics (MQTT):

   *enable applications and devices to get, update, or delete the state information for a device.*

   - the names of these topics start with $aws/things/thingName/shadow.

   - topic-based authorization is required for publishing and subscribing.
   - AWS IoT reserves the right to add new topics to the existing topic structure.

2. Device shadow state:

   *both a desired and reported state for the device in question.*

3. Device shadow metadata:

   *information about the data stored in the state section of the document.*

   - timestamps, in Epoch time, for each attribute in the state section

**Lifecycle of Device Shadow:**

1. Device publishes current state

2. Persist JSON data store

3. App requests device's current state

4. App requests change the state

5. Device Shadow syncs updated state

   ```json
   {
       "state": {
           "desired": {
               "lights":{"color": "RED"},
               "engine": "ON"
           },
           "reported":{
               "lights":{"color": "GREEN"},
               "engine":"ON"
           },
           "delta":{
               "lights":{"color": "RED"}
           }
       },
       "version": 10,
       "clientToken": "UniqueClientToken",
       "timestamp":
   }
   ```

   *(Because there is a difference between the desired state and the reported state, the service sends a message to the `delta` topic for the shadow asking it to turn the lights as 'RED')*

6. Device publishes current state

7. Device Shadow confirms state change

8. Device Shadow sends a message to the application that the update was successfully performed.

   *you need to be subscribed to several topics below to receive the message from the shadow service.*

   - /update/documents
   - /update/accepted/
   - /update/delta

**Device shadow MQTT topics**

UPDATE:

`$aws/things/thingName/shadow/update`

```json
{
    "state": {
        "desired":{
            "speed": 65,
            "engine": "on"
        }
    }
}
```

UPDATE updates the values of the state variable. If the state variable did not exist previously, it will be created. The data is stored with timestamp information to indicate when it was last updated. Messages are sent to all subscribers with the difference between desired or reported state(delta). Things or apps that receive a message can perform an action based on this difference.

Updates affect only the fields specified in the request.

GET:

- Publish:`$aws/things/thingName/shadow/get`
- Subscribe: `$aws/things/thingName/shadow/get/accepted`

```json
{
    "state": {
        "reported":{
            "lights": {"color": "green"},
        }
    },
    "metadata":{
        "reported":{
            "lights": {
                "color": {
                    "timestamp": 12345678
                ...
```

GET retrieves the latest state stored in the device's shadow. This method returns the full JSON document, including metadata, plus the delta between the desired or reported states.

DELETE

`$aws/things/thingName/shadow/delete`

DELETE deletes a device's shadow, including all of its content. This removes the JSON document from the data store. You cant restore a device's shadow you deleted, but you can create a new shadow with the same name.

And field with a value of null is removed from the document.

DELTA

`$aws/things/thingName/shadow/update/delta`

```json
{
    "messageNumber": 1,
    "payload": {
        "version": 2,
        "timestamp": 1469564658,
        "state": {
            "color": "green"
        },
        "metadata":{
            "color": {
                "timestamp": 1469564658
            }
        }
    },
    "qos": 0,
    "timestamp": 1469564658,
    "topic": "$aws/things/myLightBulb/shadow/update/delta"
}
```

Delta state is a virtual type of state that contains the difference between the desired and reported states. The values of metadata in delta are equal to the metadata in the desired field.

The Device Shadow service calculates the delta by iterating through each field in the desired state and comparing it to the reported state.
