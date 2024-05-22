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
```bash
docker run --name quest2ros --rm -it --net host tge1375/quest2ros2:0.0.1
# Or, If you have built container
docker run --name quest2ros --rm -it --net host ros-bridge-container

```

* Usage Example - cmd_vel pub/sub

```bash
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

```bash
# Terminal 1 - Build Pkgs && Docker ROS 1 core
source /opt/ros/noetic/setup.bash
cd /home/user/quest2ros_ws/src/ROS-TCP-Endpoint/src/ros_tcp_endpoint
vi default_server_endpoint.py
# edit => #!/usr/bin/env python3
cd /home/user/quest2ros_ws/
catkin build
source devel/setup.bash

roscore

# Terminal 2 - Docker ROS 1 ros_tcp_endpoint
source /opt/ros/noetic/setup.bash
source devel/setup.bash
# check your ip config through "ifconfig"
roslaunch ros_tcp_endpoint endpoint.launch tcp_ip:=<your-ip-port> tcp_port:=10000

# Terminal 3 - Docker ROS 1 quest2ros
source /opt/ros/noetic/setup.bash
source devel/setup.bash
rosrun quest2ros ros2quest.py

# Terminal 4 - Docker ROS 2 bridge
source /opt/ros/noetic/setup.bash
source /opt/ros/foxy/setup.bash 
ROS_DOMAIN_ID=30 ros2 run ros1_bridge dynamic_bridge --bridge-all-topics

# Terminal 5 - "LOCAL" ROS 2 sub check
$ ros2 topic echo
/q2r_twist
/dice_twist
```
