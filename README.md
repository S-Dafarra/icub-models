# icub-models

Repository containing models [automatically](https://github.com/robotology-playground/icub-model-generator/blob/master/.travis.yml#L76) generated from the CAD file by [icub-model-generator](https://github.com/robotology-playground/icub-model-generator).

## Usage

The model in the repo can be used either directly from the repo, or by installing them.

While the files can be used directly by pointing your software to their location, they are
tipically used by software that uses either YARP, ROS or Gazebo. For this reason, the models
are installed as part of the `iCub` ROS package ([instructions](https://github.com/gerkey/ros1_external_use#installing-for-use-by-tools-like-roslaunch)) and following the [YARP guidelines on installing configuration files](http://www.yarp.it/yarp_data_dirs.html).

To make sure that this models are found by the software even when they are not installed in
system directories, tipically the [`YARP_DATA_DIRS`](http://www.yarp.it/yarp_data_dirs.html), 
[`ROS_PACKAGE_PATH`](http://wiki.ros.org/ROS/EnvironmentVariables#ROS_PACKAGE_PATH), and the [`GAZEBO_MODEL_PATH`](http://gazebosim.org/tutorials?tut=components#EnvironmentVariables) enviromental variables are modified appropriatly.


### From the source repo

In the case models are used from the repo, the first step is configure it with the following commands:

```sh
mkdir build
cd build
cmake ..
```

If `<icub-models>` is the location of the repo, some folders need to be appended to the mentioned env variables. On *nix system, this can be achived by adding to the `.bashrc` or equivalent file the following three lines:

```sh
export YARP_DATA_DIRS=${YARP_DATA_DIRS}:<icub-models>/build/iCub
export ROS_PACKAGE_PATH=${ROS_PACKAGE_PATH}:<icub-models>/build
```

### By installing the models

To install the models instead, execute:

```sh
mkdir build
cd build
cmake -DCMAKE_INSTALL_PREFIX=<prefix> ..
cmake --build . --target install
```

Once the models are installed into a given prefix, edit the env variables as follows:

```sh
export YARP_DATA_DIRS=${YARP_DATA_DIRS}:<prefix>/share/iCub
export ROS_PACKAGE_PATH=${ROS_PACKAGE_PATH}:<prefix>/share
```
### Use the models with Gazebo
In order to use these models in Gazebo, set up the simulation environment following the instructions provided in the [icub-gazebo](https://github.com/robotology/icub-gazebo) repository, and add the following line to your ``.bashrc``:
```
export GAZEBO_MODEL_PATH=${GAZEBO_MODEL_PATH}:<prefix>/share/iCub/robots:<prefix>/share
```
Note that it is still a **Work In Progress**. See the issue https://github.com/robotology-playground/icub-models/issues/7

## Change the orientation of the root frame
The iCub robot `root frame` is defined as [`x-backward`][1], meaning that the x-axis points behind the robot. Nevertheless, in the robotics community, sometimes the root frame of a robot is defined as [`x-forward`][2]. As a consequence, to use the iCub models with software developed for the `x-forward` configuration (e.g. [IHMC-ORS][3]), might be necessary to quickly update the root frame orientation.  
For this purpose, locate the joint `<joint name="base_fixed_joint" type="fixed">` in the `URDF` model and perform the following substitution in the `origin` section:

```
-  <origin xyz="0 0 0" rpy="0 -0 0"/>
+  <origin xyz="0 0 0" rpy="0 -0 3.14159265358979323846"/>
```

[1]:http://wiki.icub.org/wiki/ICubForwardKinematics
[2]:http://www.ros.org/reps/rep-0103.html#axis-orientation
[3]:https://github.com/ihmcrobotics/ihmc-open-robotics-software
