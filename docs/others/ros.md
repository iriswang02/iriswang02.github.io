

# ROS



### Graph Concepts

**Nodes**: A node is an executable file within a ROS package that uses a ROS client library to communicate with other nodes.

- Nodes can publish or subscribe to a Topic.
- Nodes can also provide or use a Service.

*Client Libraries: 1. rospy = python client library. 2. roscpp = c++ client library.*

**Messages**: ROS data type used when subscribing or publishing to a topic.

**Master**: Name service for ROS 

**rosout**: ROS equivalent of stdout/stderr

**roscore**: Master+rosout+parameter server (parameter server will be introduced later)

roscore is the first thing you should run when using ROS.

```shell
$ roscore
```

You will see something similar to:

```shell
... logging to ~/.ros/log/9cf88ce4-b14d-11df-8a75-00251148e8cf/roslaunch-machine_name-13039.log
Checking log directory for disk usage. This may take awhile.
Press Ctrl-C to interrupt
Done checking log file disk usage. Usage is <1GB.

started roslaunch server http://machine_name:46021/
ros_comm version 1.12.14

SUMMARY
======

PARAMETERS
 * /rosdistro: kinetic
 * /rosversion: 1.12.14

NODES

auto-starting new master
process[master]: started with pid [5124]
ROS_MASTER_URI=http://machine_name:11311/

setting /run_id to 9cf88ce4-b14d-11df-8a75-00251148e8cf
process[rosout-1]: started with pid [5137]
started core service [/rosout]
```

Keep this terminal open and open up a new terminal

```shell
$ rosnode list
```

This displays information about the ROS nodes that are currently running. 

```shell
/rosout
```

This showed us that there is only one node running: rosout. This is always running as it collects and logs nodes' debugging output.

```shell
$ rosnode info /rosout
```

This gives us some more information about rosout

```shell
------------------------------------------------------------------------
Node [/rosout]
Publications:
 * /rosout_agg [rosgraph_msgs/Log]

Subscriptions:
 * /rosout [unknown type]

Services:
 * /rosout/get_loggers
 * /rosout/set_logger_level

contacting node http://machine_name:44127/ ...
Pid: 5219
```



**rosrun**: rosrun allows you to use the package name to directly run a node within a package (without having to know the package path).

```shell
$ rosrun turtlesim turtlesim_node
```

![turtlesim.png](http://wiki.ros.org/ROS/Tutorials/UnderstandingNodes?action=AttachFile&do=get&target=turtlesim.png)

In a new terminal:

```shell
$ rosnode list
```

```shell
/rosout
/turtlesim
```

One powerful feature of ROS is that you can reassign Names from the command-line.

```shell
$ rosrun turtlesim turtlesim_node __name:=my_turtle
```

```shell
$ rosnode list
```

```shell
/my_turtle
/rosout
```



```shell
$ rosnode ping my_turtle
```

```shell
rosnode: node is [/my_turtle]
pinging /my_turtle with a timeout of 3.0s
xmlrpc reply from http://aqy:42235/     time=1.152992ms
xmlrpc reply from http://aqy:42235/     time=1.120090ms
xmlrpc reply from http://aqy:42235/     time=1.700878ms
xmlrpc reply from http://aqy:42235/     time=1.127958ms
```



**Topics**: Nodes can publish messages to a topic as well as subscribe to a topic to receive messages.

**rqt_graph**: It creates a dynamic graph of what's going on in the system. rqt_graph is part of the `rqt` package.

```shell
$ rosrun rqt_graph rqt_graph
```

![rqt_graph_turtle_key.png](http://wiki.ros.org/ROS/Tutorials/UnderstandingTopics?action=AttachFile&do=get&target=rqt_graph_turtle_key.png)

 As you can see, the `turtlesim_node` and the `turtle_teleop_key` nodes are communicating on the topic named `/turtle1/command_velocity`.

**rostopic**: It allows you to get information about ROS topics.

```shell
$ rostopic echo /turtle1/cmd_vel
```

Let's make `turtle_teleop_key` publish data by pressing the arrow keys.

```shell
linear: 
  x: 2.0
  y: 0.0
  z: 0.0
angular: 
  x: 0.0
  y: 0.0
  z: 0.0
---
linear: 
  x: 2.0
  y: 0.0
  z: 0.0
angular: 
  x: 0.0
  y: 0.0
  z: 0.0
---
```

Now let's look at `rqt_graph` again. As you can see `rostopic echo`  is now also **subscribed** to the `turtle1/command_velocity` topic.![rqt_graph_echo.png](http://wiki.ros.org/ROS/Tutorials/UnderstandingTopics?action=AttachFile&do=get&target=rqt_graph_echo.png)



```shell
$ rostopic list -v
```

This displays a verbose list of topics to publish to and subscribe to and their type.

```shell
Published topics:
 * /turtle1/color_sensor [turtlesim/Color] 1 publisher
 * /turtle1/cmd_vel [geometry_msgs/Twist] 1 publisher
 * /rosout [rosgraph_msgs/Log] 2 publishers
 * /rosout_agg [rosgraph_msgs/Log] 1 publisher
 * /turtle1/pose [turtlesim/Pose] 1 publisher

Subscribed topics:
 * /turtle1/cmd_vel [geometry_msgs/Twist] 1 subscriber
 * /rosout [rosgraph_msgs/Log] 1 subscriber
```



```shell
$ rostopic type /turtle1/cmd_vel
```

This returns the message type of any topic being published.

```shell
geometry_msgs/Twist
```

The details of the message

```shell
$ rosmsg show geometry_msgs/Twist
```

```shell
geometry_msgs/Vector3 linear
  float64 x
  float64 y
  float64 z
geometry_msgs/Vector3 angular
  float64 x
  float64 y
  float64 z
```



```shell
$ rostopic pub -1 /turtle1/cmd_vel geometry_msgs/Twist -- '[2.0, 0.0, 0.0]' '[0.0, 0.0, 1.8]'
```

The previous command will send a single message to `turtlesim` telling it to move with a linear velocity of 2.0, and an angular velocity of 1.8 .

![turtle(rostopicpub).png](http://wiki.ros.org/ROS/Tutorials/UnderstandingTopics?action=AttachFile&do=get&target=turtle%28rostopicpub%29.png)

The turtle requires a steady stream of commands at 1 Hz to keep moving. We can publish a steady stream of commands using `rostopic pub -r` command:

```shell
$ rostopic pub /turtle1/cmd_vel geometry_msgs/Twist -r 1 -- '[2.0, 0.0, 0.0]' '[0.0, 0.0, -1.8]'
```

![turtle(rostopicpub)2.png](http://wiki.ros.org/ROS/Tutorials/UnderstandingTopics?action=AttachFile&do=get&target=turtle%28rostopicpub%292.png)

We can also look at what is happening in `rqt_graph`. The rostopic pub node is communicating with the rostopic echo node.

![rqt_graph_pub.png](http://wiki.ros.org/ROS/Tutorials/UnderstandingTopics?action=AttachFile&do=get&target=rqt_graph_pub.png)



```shell
$ rostopic hz /turtle1/pose
```

This reports the rate at which data is published.

```
subscribed to [/turtle1/pose]
average rate: 59.354
        min: 0.005s max: 0.027s std dev: 0.00284s window: 58
average rate: 59.459
        min: 0.005s max: 0.027s std dev: 0.00271s window: 118
average rate: 59.539
        min: 0.004s max: 0.030s std dev: 0.00339s window: 177
average rate: 59.492
        min: 0.004s max: 0.030s std dev: 0.00380s window: 237
average rate: 59.463
        min: 0.004s max: 0.030s std dev: 0.00380s window: 290
```

Now we can tell that the turtlesim is publishing data about our turtle at the rate of 60 Hz.



**rqt_plot** : It displays a scrolling time plot of the data published on topics.

```shell
$ rosrun rqt_plot rqt_plot
```

![rqt_plot.png](http://wiki.ros.org/ROS/Tutorials/UnderstandingTopics?action=AttachFile&do=get&target=rqt_plot.png)



**Services**: Another way that nodes can communicate with each other. Services allow nodes to send a request and receive a response.

```shell
$ rosservice list
```

```shell
/clear
/kill
/reset
/rosout/get_loggers
/rosout/set_logger_level
/spawn
/teleop_turtle/get_loggers
/teleop_turtle/set_logger_level
/turtle1/set_pen
/turtle1/teleport_absolute
/turtle1/teleport_relative
/turtlesim/get_loggers
/turtlesim/set_logger_level
```



```shell
$ rosservice type /clear
```

```shell
std_srvs/Empty
```

This service is empty, this means when the service call is made it takes no arguments.



```shell
$ rosservice call /clear
```

It clears the background of the `turtlesim_node`.



```shell
$ rosservice type /spawn | rossrv show
```

```shell
float32 x
float32 y
float32 theta
string name
---
string name
```

This service lets us spawn a new turtle at a given location and orientation.

```shell
$ rosservice call /spawn 2 2 0.2 ""
```

The service call returns with the name of the newly created turtle

```shell
name: "turtle2"
```

![turtle(service).png](http://wiki.ros.org/ROS/Tutorials/UnderstandingServicesParams?action=AttachFile&do=get&target=turtle%28service%29.png)



**rosparam** allows you to store and manipulate data on the ROS Parameter Server. The Parameter Server can store integers, floats, boolean, dictionaries, and lists. `rosparam` uses the YAML markup language for syntax.



```shell
$ rosparam list
```

```shell
/background_b
/background_g
/background_r
/rosdistro
/roslaunch/uris/host_57aea0986fef__34309
/rosversion
/run_id
```



Here will change the red channel of the background color:

```shell
$ rosparam set /background_r 150
```

This changes the parameter value, now we have to call the clear service for the parameter change to take effect:

```shell
$ rosservice call /clear
```

![turtle(param).png](http://wiki.ros.org/ROS/Tutorials/UnderstandingServicesParams?action=AttachFile&do=get&target=turtle%28param%29.png)

This gets the value of the green background channel:

```shell
$ rosparam get /background_g 
```

```shell
86
```

 This shows the contents of the entire Parameter Server.

```shell
$ rosparam get /
```

```shell
background_b: 255
background_g: 86
background_r: 150
roslaunch:
  uris: {'aqy:51932': 'http://aqy:51932/'}
run_id: e07ea71e-98df-11de-8875-001b21201aa8
```



Here we write all the parameters to the file params.yaml

```shell
$ rosparam dump params.yaml
```



- msg: msg files are simple text files that describe the fields of a ROS message. They are used to generate source code for messages in different languages.
- srv: an srv file describes a service. It is composed of two parts: a request and a response.



