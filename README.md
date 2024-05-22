# quest_ros2_docker
ROS 2 docker for using Meta Oculus Quest as robot controller

## Setup Guidance 


* build dockerfile
```
git clone https://github.com/kimsooyoung/quest_ros2_docker.git
cd quest_ros2_docker

docker build Docker -t ros-bridge-container
```

> you can also run fully prepared docker image from my docker hub!
> `docker pull tge1375/quest2ros2:0.0.1`

* Run Container
```
docker run --name quest2ros --rm -it --net host tge1375/quest2ros2:0.0.1
# Or, If you have built container
docker run --name quest2ros --rm -it --net host ros-bridge-container

```

* Usage Example - cmd_vel pub/sub

```
# Terminal 1 - Docker ROS 1 core
source /opt/ros/noetic/setup.bash
roscore

# Terminal 2 - Docker ROS 1 pub
source /opt/ros/noetic/setup.bash
rostopic pub --rate 1 -s /cmd_vel geometry_msgs/Twist "linear:
  x: 2.0
  y: 0.0
  z: 0.0
angular:
  x: 0.0
  y: 0.0
  z: 1.8"

# Terminal 3 - Docker ROS 2 bridge
source /opt/ros/noetic/setup.bash
source /opt/ros/foxy/setup.bash 
ROS_DOMAIN_ID=30 ros2 run ros1_bridge dynamic_bridge --bridge-all-topics

# Terminal 4 - "LOCAL" ROS 2 sub 
ros2 topic echo /cmd_vel
```

* Usage Example - Quest2ROS 

* check your ip config and then execute `ros_tcp_endpoint`

```
roslaunch ros_tcp_endpoint endpoint.launch tcp_ip:=192.168.0.10 tcp_port:=10000
```