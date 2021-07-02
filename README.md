# where_am_I_robot_localization

This is a projected made for learning and implementing Monte Carlo localization (aka particle filter) for the Udacity Robotics Nanodegree program. This project utilizes existing __ROS__ package to create a map of a user created world and localize the robot in the environment. 

# Quickstart

## Prerequisite

Create a new catkin workspace (generally named as catkin_ws) and a src folder within it . Example `mkdir -p catkin_ws\src`. (new catkin workspace is preferred in order to avoid conflicts with other existing catkin repositories)

Navigate into the src folder. `cd catkin_ws/src` and run the `catkin_init_workspace` command to initialize the catkin workspace.

Clone the git repository. ` git clone https://github.com/sarkrishnan/where_am_I_robot_localization.git`

Navigate into the catkin workspace. `cd catkin_ws` and run `catkin_make` command to build the repository

Source the catkin_ws `source devel/setup.bash` and run `roslaunch my_robot world.launch`. You should see gazebo and rviz (another visualization tool) open up and the gazebo simulation will show a building with some rooms and table and a robot. 

### Setting up rviz

In Rviz,

* Select odom for fixed frame
* Click the “Add” button and
  * add RobotModel: this would add the robot itself to RViz.
  * add Map and select first topic/map: the second and third topics in the list will show the global costmap, and the local costmap. Both can be helpful to tune your parameters (the topic/map will be available only after creating the map and running the amcl.launch file)
  * add PoseArray and select topic/particlecloud: this will display a set of arrows around the robot (the topic will be available only after we run the amcl.launch file later). 

Save the configuration for future use Click __File -> save config__ (this will make it the default config for rviz)


## Creating map 

We need *libignition-math2-dev* and *protobuf-compiler* to compile the map creator `sudo apt-get install libignition-math2-dev protobuf-compiler`

Navigate to the source folder. `cd catkin_ws/src`

Clone the pgm_map_creator github repository. `https://github.com/udacity/pgm_map_creator.git` (Refer to the README file in the repository on how to create the map, essentially copy the world file from the above cloned *where_am_I_robot_localization/my_robot/world* folder into the world folder of pgm repo and create the map following the instrucation in the README file of pgm repo)

_Note: If you create your own world, you might need to edit in the request_publisher.launch file arg values (xmin, xmax, ymin, ymax) to ensure that the map encompasses the world fully._

Create a maps folder within the my_robot folder. Navigate into myrobot and run command `mkdir maps`.

Copy the above created map (from pgm_map_creator/maps folder) into the new maps (to my_robot/maps) folder.  (use `cp <source> <destination>` command). 

Within the myrobot/maps folder, create a map.yaml file with your favorite editor (gedit/nano/...) ex: `gedit map.yaml` and copy the following content
```
image: <YOUR MAP NAME>
resolution: 0.01
origin: [-15.0, -15.0, 0.0]
occupied_thresh: 0.65
free_thresh: 0.196
negate: 0
```
_Note: The origin needs to be changed if you modify the request_publisher.launch file_

## Running the localization ros package

Run `roslaunch my_robot world.launch`. You should see gazebo and rviz (another visualization tool) open up

Run `roslaunch my_robot amcl.launch` to run the monte carlo localization. You can use the `Navigate 2D goal` button in rviz to set navigation goal to the robot. Initially it might take 10-30 seconds to launch the map properly and start the localization algorithm. Choose the topics in rviz to get the proper visualization. 

Congratulations

## Suggestions for improvements/tinkering

Try to navigate the robot using [robot-teleop](https://github.com/ros-teleop/teleop_twist_keyboard) instead of move base package which I am currently utilizing

Try to fiddle with the monte carlo localization package [parameter](http://wiki.ros.org/amcl#Parameters).

### Reference/Source attribution

I have made this README file by generously consulting and borrowing from the Udacity project guide. Please enroll in the Robotics Software Engineer Nanodegree program if you want to learn/refresh your ROS skills. 



