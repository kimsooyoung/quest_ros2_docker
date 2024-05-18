# quest_ros2_docker
ROS 2 docker for using Meta Oculus Quest as robot controller

## Setup Guidance 


* build dockerfile
```
git clone https://github.com/kimsooyoung/quest_ros2_docker.git
cd quest_ros2_docker

docker build Docker -t ros-noetic-container
```

> you can also run fully prepared docker image from my docker hub!
> `docker pull tge1375/`

* run docker
```
docker run -it --rm --user=$(id -u $USER):$(id -g $USER) --env="DISPLAY" --volume="/etc/group:/etc/group:ro" --volume="/etc/passwd:/etc/passwd:ro" --volume="/etc/shadow:/etc/shadow:ro" --volume="/etc/sudoers.d:/etc/sudoers.d:ro" --net host -v /home:/home -v ~/Volumes:/home/usr/ ros-noetic-container
```



* 