# bwi

The Building Wide Intelligence (BWI) is a project of the University of Texas at Austin to develop fully autonomous mobile robots that can become a permanent part of a building’s environment. 
The BWI project provides ROS packages, which are maintained on Github. 


## BWI Repository Hierarchy

One of the repositories, bwi, contains top-level ROS packages for the BWI project so that the repository can maintain packages contained in various released BWI repositories, which may depend on other packages at the same or lower levels.

From top to bottom, the released repositories are:

 * [bwi](http://wiki.ros.org/bwi)
 * [segbot](http://wiki.ros.org/segbot)
 * [bwi_common](http://wiki.ros.org/bwi_common)

## System Specification
```
Ubuntu 18.04
ROS version: melodic
Python version: 2.7 (ROS melodic does not support python 3, however, you can use python 3 code-base on python 2)
```

## Installation
You can find the documentation of installation maintained for the AIR Lab [here](https://docs.google.com/document/d/1yAzUIvP_1WJ0NI6G4xmJ8DqDjguHFxHpCKpSC1FQW28/edit#heading=h.lg96moxe7p4w).


### From Source

You can install all the BWI components normally built from source on
either ROS Indigo, Kinetic, or Melodic.  For the V4 bots, use Melodic.

First, install ROS
[Indigo](http://wiki.ros.org/indigo/Installation/Ubuntu),
[Kinetic](http://wiki.ros.org/kinetic/Installation/Ubuntu), or
[Melodic](http://wiki.ros.org/melodic/Installation/Ubuntu).
The Kinetic version is only supported on Ubuntu Xenial, and is
only partially functional.

Then, make sure the ROS_DISTRO environment variable is set correctly:

```
echo $ROS_DISTRO
```

It may already be.  If not, issue the appropriate one of these two
shell commands:

```
$ export ROS_DISTRO=kinetic
```
or
```
$ export ROS_DISTRO=melodic
```

Next, clone the source repositories:
```
$ source /opt/ros/melodic/setup.bash
$ mkdir -p ~/segbot_ws/src
$ cd ~/segbot_ws
$ wstool init src https://raw.githubusercontent.com/YoheiHayamizu/bwi/bu-devel/rosinstall/melodic.rosinstall
```

Install all dependencies:
```
$ rosdep update
$ rosdep install --from-paths src --ignore-src --rosdistro $ROS_DISTRO -y
```


**If you are setting up a workspace on a BWIbot V4 in Anna Hiss Gym, continue from here with the "Special Installation Instructions for Version 4 Robots" below.**


Then, build everything. On a slow computer:
```
$ catkin build -j2 
$ source devel/setup.bash
```

Or on a fast computer:
```
$ catkin build -j6 
$ source devel/setup.bash
```
Or you can adjust the job limit as you see fit.


Note that the **catkin build** command from the **python-catkin-tools**
package is *required* for building on ROS Kinetic and Melodic. On ROS Indigo, you
can still use **catkin_make** instead, although the newer build tool
is recommended.


## Special Installation Instructions for Version 4 Robots:

The V4 bots at AHG use some additional branches of the utexas-bwi repo.  To install these, run the following commands from your ~/catkin_ws directory (they will change directories):

```
$ roscd bwi_common && git fetch && git checkout architecture-update
$ git submodule init
$ git submodule update

$ cd ~/catkin_ws/src && git clone --branch ahg2s_map https://github.com/utexas-bwi/ahg_common.git
```

Then build everything from the ~/catkin_ws directory:
```
$ cd ~/catkin_ws
$ catkin build -j6
```

The V4 base uses slightly different values for some environment variables. It's important to export them into your environment after sourcing the workspace. To automate that, add the convenience function "sws" to the bottom of your .bashrc file found at ~/.bashrc.
```
sws() {
  source ~/catkin_ws/devel/setup.bash
  export SEGWAY_INTERFACE_ADDRESS=10.66.171.1
  export SEGWAY_IP_ADDRESS=10.66.171.5
  export SEGWAY_IP_PORT_NUM=8080
  export SEGWAY_BASE_PLATFORM=RMP_110
  export SEGWAY_PLATFORM_NAME=RMP_110
}
sws
```
After building your workspace or packages, instead of using "source devel/setup.bash" , simply call the function above by typing:
```
$ sws
```

Next, update pyYAML:
```
$ pip install -U pyYAML
```

Then you will need to create a password for postgres.  To do so, run the script below and follow the promts to enter a password that is not empty:
```
$ ./src/bwi_common/knowledge_representation/scripts/configure_postgresql.sh
```

Finally, run the command prepare_knowledge_bwi_ahg:
```
$ prepare_knowledge_bwi_ahg
```

At this point, you should be able to run the BWIbot V4 visit doors demo in Anna Hiss Gym.  See the [demo instructions](https://github.com/utexas-bwi/bwi/blob/master/demo_v4.md) in this directory.
