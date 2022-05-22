# tw-robot
Ros demo for beginners

# Description 
This demo is for beginners who just started learning Ros. You will learn how to create a robot model and insert plugins to work in gazebo. Apart from that you will see how packages teleop and gmapping work. At the end you will have a map generated in rviz. Happy learning.

# Create a catkin workspace
You have to have Ros kinetic-kame installed. Navigate to the directory you want to work with and open in terminal. Then execute the following commands.
```
mkdir -p ws_twrobot/src
cd src/
catkin_init_workspace

```
# Clone the package into your source folder
```
git clone https://github.com/AsiriSirithunga/tw-robot.git
```
Now you have the created package and you are good to move forward. Install the system dependencies. 

```
cd ..
rosdep update
rosdep install --from-paths src --ignore-src --rosdistro=kinetic -y
```
For and example you can install gmapping package by executing the command below.

```
sudo apt-get install ros-kinetic-gmapping
```

# Build and source the workspace
When you install all the dependencies you will see this text ' #All required rosdeps installed successfully'. It's time to build the package.

```
catkin build
source devel/setup.bash
```
# Start gazebo and rviz
Launch rviz.
```
roslaunch tw-robot twrobot_base_rviz.launch
```
You can visualize the robot model and the environment in gazebo. First, terminate the rviz. Then execute the command below.

```
roslaunch tw-robot twrobot_base_rviz_gazebo.launch
```

# Teleop demo
Using this package you can move the robot. First open the rviz and gazebo. Then open a new terminal in the workspace. Then execute the commands below.
```
source devel/setup.bash
roslaunch tw-robot twrobot_control_teleop.launch
```

While opening the terminal you can control the robot using keyboard.

# Gmapping demo
In this stage you can generate a map of the gazebo world and load the same map into rviz. Open a new terminal and execute the commands below.

``` 
source devel/setup.bash
roslaunch tw-robot slam_gmapping.launch
```
After exploring the whole world you can save the generated map in the yaml format using the following commands. Open a new terminal and navigate to the maps folder inside the tw-robot package. Execute the command below to save the map.
```
rosrun map_server map_saver -f sample_room
```
Then add the below node into the rviz gazebo launch file. 
```
<!--Map Server-->
  <node pkg="map_server" type="map_server" name="map_server" args="$(find tw-robot)/maps/sample_room.yaml" respawn="false">
    <param name="frame_id" value="/odom" />
</node>
```
# Amcl demo
This package is for localizing the robot model in the saved map. To launch it. Execute the commands below in a new terminal after succesful launching of rviz and gazebo with the saved map.

```
source devel/setup.bash
roslaunch tw-robot amcl.launch
```
# Move Base demo
Using this package you can autonomously navigate the robot to a 2d navigation goal while avoiding obstacles in the environment. The package needs the mapserver run. It will publish velocity commands to the cmd_vel topic. The parameters must tune with respect to the application in advance to better performance. Execute the commands below in a new terminal after succesful launching of rviz and gazebo with the saved map.

```
source devel/setup.bash
roslaunch tw-robot move_base.launch
```

Great work! Now you can use different packages upon this to learn more. 
