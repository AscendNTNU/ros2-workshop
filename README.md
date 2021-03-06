# ros2-workshop
ROS 2 workshop for Ascend Team 2022

## What is ROS?

![ROS foxy](https://docs.ros.org/en/foxy/_static/foxy-small.png)


## Installation (Ubuntu 20.04)
> Note: Installation for MacOS down below.

This workshop is built on Ubuntu 20.04. Make sure that you are running the same distribution before continuing. 
Check your current version by typing the following into your terminal.
```
lsb_release -a
```
This should reveal your Ubuntu distro. 

### Step 1: Cloning the repository
> Prerequesites: You should already have set up github with ssh-keys.
Clone the repository in your home directory.

```
git clone git@github.com:AscendNTNU/ros2-workshop.git 
```
If you have set up your ssh-keys correctly, this should download the ros2-workshop directory.

### Step 2: Setting up our workspace

#### Installing docker
> Note: If you already have docker installed, you can skip this sub-step.
 
To install all the necessary packages for the workshop, you first need to give your user permission
to excecute the installation scripts.

```
cd  ~/ros2-workshop
```
```
pwd
```
This will basicly print out where you are on your computer, you should now be in the ros2-workshop directory. The next lines will give our installation scripts access to install and excecute packages.
```
sudo chmod +x scripts/dockerInstall.sh
```
```
sudo chmod +x scripts/vscodeInstall.sh
```
We now excecute the installation scripts.
```
./scripts/dockerInstall.sh
```
We now test if everything was installed successfully, but first we need to source our new enviroment variables. 
```
source ../.bashrc
```
```
docker run docker/whalesay cowsay Docker install completed
```
You should now see a very cute whale printed in your terminal.

#### Installing VSCode
> Note: If you have VSCode installed on your machine, you can skip the next step.

```
./scripts/vscodeInstall.sh
```

### Step 3: Launching the workspace
Open ros2-workshop in VSCode and make sure to install the extensions in the pop-up that should appear in you bottom-right corner. 
After installing the extensions for VSCode, you should reload your window. Ctrl + Shift + P should open your VSCode command palette.
Excecute "Reload Window". You should now see a prompt in your bottom left that should say "Reopen in Containter". The same can be achieved by 
excecuting "Remote-Containers: Reopen in Container" in the VSCode command palette. Your window should now reload, and launch your ros-workspace in
an isolated container. We can now begin programming with ROS2.
> Note: While viewing `README.md` in VSCode, press Ctrl + Shift + V to view the file in beautiful markdown format :)

> Note: If you don't like using the terminal in VSCode, you can attach to the container with `docker exec -it ros2_ws_container bash` in another shell. 


<details>
  <summary>For the curious: Containers</summary>
  Our current workspace is now running in a container, but what is a container? A container is a filesystem isolated from your root filesystem. We achieve this by restricting all the processes inside to this filesystem. From programs inside a container can only access the resources and files allocated to it. It is in some sense a virtual machine, but is sharing the resources with the host machine. If you want to learn more, we reccomend this video https://www.youtube.com/watch?v=8fi7uSYlOdc. 
 
</details>

## Installation (MacOS)
Install:
* https://www.xquartz.org/
* https://docs.docker.com/desktop/mac/install/

Xquartz needs to have the following configuration.

<img src="media/xquartz_config_macos.png" alt="xquartz config" width="300"/>


Then clone the repository, and open in VSCode
```
git clone git@github.com:AscendNTNU/ros2-workshop.git 
```
Go to `.devcontainer/devcontainer.json` to comment and uncomment some environment variables that are MacOS specific, and follow the workshop from Step 3.
> Warning: Docker and Xquartz must be running while working on ros2-workshop. 

## Assignment 1: "Hello World!"
> Note: Make sure you are in the container you ran in the previous section. 

 ROS 2 relies on the notion of combining workspaces using the shell environment. Every ROS project has a workspace. ???Workspace??? is a ROS term for the location on your system where you???re developing with ROS 2. You can have several workspaces on your machine, but we'll only use one. This feature enables easy switching between different versions of ROS. This is accomplished by sourcing setup files, which include ROS environment variables. **Without sourcing ROS setup files you wont be able to access any ROS2 commands.** 

 ```
 source /opt/ros/foxy/setup.bash
 ```
 Not sourcing setup files is one of the most common mistakes when working with ROS in a shell. The ros2-workshop repo is a ROS2 workspace, and is the root for all the packages we will build in this workshop. The `src` directory contains all our source files including package information about what we want to build. When we build our workspace, the ROS build tool `colcon` will generate the necessary files and directories to run our packages. We will now build our workspace to see what `colcon` generates. In VSCode you can do this with Ctrl + Shift + B, or in the terminal with


```
colcon build --merge-install
```
> Note: The --merge-install is just a convenience argument, and is not that important for us to understand

<details>
  <summary>For the curious: Colcon</summary>
  https://colcon.readthedocs.io/en/released/reference/verb/build.html
</details>


You can now see several new directories in the root directory of our workspace. The `install` directory contains the `setup.bash` files that include the workspace environment variables we need to run our packages. Source this file with the same syntax as we used when sourcing ROS setup files. We should now be able to run our example code.
```
ros2 run assignment1 hello_world
```
You should now have a node printing out "Hello World" in your terminal. 
> Note 1: Ctrl + C to terminate the program

> Note 2: Ctrl + Shift + z suspends a process. `fg` brings it to the foreground and `bg` lets the process continue in the background. Use `ps` to view current jobs in your active terminal. 



## Assignment 2: Nodes and Topics



The `hello_world` program in assignment 1 is a ROS `node`. Every ROS program is a node, communicating in a graph of other nodes. A node can both receive and send messages. When `hello_world` prints out a message, every other node can chose to listen to `hello_world`s message. A message is sent to communication channels which in ROS are called `topic`s. Other nodes can listen and/or post messages to a `topic`. One analogy is a Slack channel. One node/user can post, listen or ignore a `topic`/channel. When a node/user posts a message to a `topic`/channel the ones subscribing to that `topic` are notified, and can read that message. A node can have a ROS `subscriber` that listens to a `topic`. The opposite is a ROS `publisher` that posts to a `topic`. 

![Graph Viz](https://docs.ros.org/en/foxy/_images/Topic-MultiplePublisherandMultipleSubscriber.gif)

Now we'll run our `hello_world` program. Open a new bash shell in VSCode ("+" icon in the top left in your current shell). 

> Note: Make sure that ROS is sourced

### Useful commands

```
ros2 node list

ros2 node info /some-node

ros2 topic list

ros2 topic info /some-topic

ros2 topic echo /some-topic

ros2 topic pub /some-topic some-msg-type some-msg
```

### Tasks
> Note: Press `tab` for autocomplete
* List all running nodes
* Echo two topics
* Post a message to the topic `hello_world` posts to



## Assignment 3: Writing a Node
### How to write a node in ROS2
 Now that you have had a quick introduction to what nodes are, we will learn how to write our own nodes in ROS2. There is a convention about how to write your nodes. You have to create a class which inherits from the Node object (for example: rclcpp::Node in Cpp, rclpy.node.Node in Python). In this class you???ll have all your ROS2 functionalities. You can then implement your case specific functionality in this class.  
 
 ```python
 class SimpleNode(Node):
    def __init__(self):
        super().__init__('my_node_name')
        # Create publishers and subscribers

    def some_callback():
      #Do something
  ```
  The constructor of the `Node` class only takes in one parameter &mdash; the name the node is to be registered with. 

  To instantiate a node, you first have to initialize ROS communications. `rclpy.init(args)` will do that for you, with some arguments that you can pass when you launch the node.




  ```python
  def main(args=None):
    rclpy.init(args=args)

    node = SimpleNode()

    rclpy.spin(node)

    rclpy.shutdown()
  ```


 `rclpy.spin(node)` will pause the program execution here, waiting for you to request to kill the node (for example CTRL+C in the terminal). During this time, any thread/timer you???ve created in the node will continue to be executed. Also, spin will be able to call any callback function that you???ve defined for the node, allowing your node to communicate with other nodes.

 When you request to kill the node, the spin function will exit, and any callback won???t be callable anymore. `rclpy.shutdown()` will basically shutdown what you started when you executed `rclpy.init()`.


 ### Writing a simple publisher 

  A timer object can be created if we want to have an action repeated periodically. We then create a timer object using the function `create_timer(period, callback)`. This will then call the callback function with the specified period. 

  A publisher is a class that can publish a message on a specific topic. In ROS2 we can create a publisher by calling the member-function `create_publisher()` function.

  
  ```python
    self.publisher = self.create_publisher(Message_type, 'topic_name')
  ```
  The constructor takes in at least two arguments &ndash; message type and topic name. Messages are divided into different message types, with individual data fields. In this way we know how the data in the message is to be interpreted. Some examples of message types are `String` and `Image`. The publisher object has a member function called `publish(message)`, which takes in a message object of the type specified in the constructor. The message object needs to be created and it's data fields populated before it is published. 

  ```python
    msg = String()
    msg.data = "Some message"
  ```
  ### Task
  You now want to create a node that publishes a message in the form of a `String`. In the `PublisherNode.py` file we have provided you with a code skeleton. Create a publisher and publish a message with a desired period. Then build and run your node and make sure everything works.
 
  <details>
  <summary>Hint: Checking if it works</summary>

  ```
  Use 'ros2 topic echo' to make sure the node is publishing the message
  ```
  </details>

   <details>
  <summary>Hint: Publishing the message</summary>

  ```
  self.publisher.publish(msg)
  ```
  </details>

  ### Writing a simple subscriber
  A subscriber is an object that listens for messages on a topic. We can create a subscriber object by calling the member function `create_subscription(Message_type, 'topic_name', callback_function)`. Unlike publishers, subscribers have a third parameter &mdash; the callback function. This function is called each time a message is received.

  ```python
 class SimpleNode(Node):
    def __init__(self):
        super().__init__('my_node_name')
        self.subscriber = self.create_subscription(msg_type, 'topic_name', self.some_callback)

    def some_callback(self, msg):
      #Do something
  ```

  ### Task
  Now finnish the subscriber in `SubscriberNode.py`. Subscribe to the same topic you published on earlier and make the subscriber print out the message.

  ### Task
  Play around with the publishing frequency. A useful tool is `ros2 topic hz topic_name`. Can you figure out what it does? 


## Assignment 4: Services and Turtlesim
### Service nodes
Messeges are published, and the sender has no idea who listens to them. The publisher also does not expect a reply. Services on the other hand &ndash; require a reply. When nodes communicate using services, the node that sends a request for data is called the client node, and the one that responds to the request is the service node.

![Graph Viz](https://docs.ros.org/en/foxy/_images/Nodes-TopicandService.gif)

```python
class ServiceNode(Node):

    def __init__(self):
        super().__init__('minimal_service')
        self.srv = self.create_service(service_type, 'service_name', self.some_service_callback)

    def some_service_callback(self, request, response):
        #Do something with request
        # ...
        # Create response based on result
        return response
```
The service is advertised by calling the `create_service()` member function. This takes three parameters &ndash; the type of service, the name the service is advertised under and the callback for the service. The type of the service can be defined by the programmer, but there are already a few services predefined. One example of such a service is `Empty`, which takes in no data in the requesr and returns a boolean value. The callback here takes two parameters &ndash; unlike the subscriber callback which takes one &ndash; the request and the response. It is important to return the response to the servivce. 


### Service clients
To call a service we first have to create the service client. This is done by calling the member function `create_client(service_type, 'service_name')`. Here we don't need to specify a callback. 


### Task
Open turtlesim by calling 
```bash
ros2 run turtlesim turtlesim_node
```
When we work with services, we have some usefull command line tools &ndash; just like we had with topics. They are as follows
```bash
ros2 service list
ros2 service type <service_name>
ros2 service find <service_type>
ros2 service info
ros2 service call <service_name> <service_type> <arguments>
```
The arguments part is optional. For example, the Empty typed services don???t have any arguments. We now want you to make the turtle move in a square pattern by calling the correct service.
<details>
 <summary>Hint: Finding the correct service</summary>

  ```
  Call ros2 service list while the turtle sim is running. 
  ```
  </details>

<details>
   <summary>Hint: Calling the service</summary>

  ```
  Remember, you can autocomplete by using tab
  ```
  </details>

## Final test
Now you need to put everything you have learned to the test. Create a package in the src directory by calling 
```bash
ros2 pkg create --build-type ament_python <package_name>
```
In this package &ndash; create a node with a service that makes the turtle move in a circle. You might need these libraries in the task:
```python
from std_srvs.srv import Trigger
from geometry_msgs.msg import Twist
from geometry_msgs.msg import Pose
```
<details>
   <summary>Hint: Moving the turtle </summary>

  ```
  Check which topics the turlesim subscribes to. 
  ```
</details>

<details> 
   <summary>Answer: Moving the turtle </summary>


  You can command the turtle with the topic /cmd_vel
 
</details>
<details>
   <summary>Hint: Movement message type </summary>

  
  Use `ros2 topic info` to view the cmd_vel message type  
  
</details>
<details>
   <summary>Kok</summary>

  
  `assignment4_kok contains a very simple service, and serves as a skeleton for finishing the task.  
  
</details>

## Conclusion 
So now you know the basics. So what's next? Coding for drones! What could possibly go wrong? 

![Oups](https://c.tenor.com/D3cahLf26vAAAAAC/drone-cat-flying.gif)

## Sources
Thirsty for more? Here are some useful links.
* https://industrial-training-dev.readthedocs.io/en/latest/
* https://docs.ros.org/en/foxy/index.html



