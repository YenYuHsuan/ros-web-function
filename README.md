# ROS tutorial - basic function for javascript

## Connect
To connect the ros server through the websocket protocol.
- `xxx.xxx.xxx.xxx` is your ROS server IP

```js
var ROS_IP = 'xxx.xxx.xxx.xxx:9090';

// Connect to ROS.
var ros = new ROSLIB.Ros({
  url : 'ws://'+ROS_IP
});
ros.on('connection', function() {
  console.log('Connected to ros server.');
});

ros.on('error', function(error) {
  console.log('Error connecting to ros server: ', error);
});

ros.on('close', function() {
  console.log('Connection to ros server closed.');
});
```

## Publisher
Publish message to the specific topic.

```js
function ros_pub(topic, msg_type, payload){
  //define the topic
  var pub_topic = new ROSLIB.Topic({
	ros : ros,
	name : topic,
	messageType : msg_type
  });
  //publish to the topic
  var pub_msg = new ROSLIB.Message(payload);
  pub_topic.publish(pub_msg);
}
```

## Subscriber
Subscribe message from the specific topic.

```js
function ros_sub(topic, msg_type){
  //define the topic
  var sub_topic = new ROSLIB.Topic({
	ros : ros,
	name : topic,
	messageType : msg_type
  });
  //subscribe from the topic
  sub_topic.subscribe(function(message) {
	console.log("Subscribe to topic " + topic + ", recieved message:" + JSON.stringify(message));
	$('#ros_subscriber').append("<p>"+JSON.stringify(message)+"</p>");
  });
}
```