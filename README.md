#CyberdogSIM

 Integrate the cyberdog_locomotion and cyberdog_simulator warehouses to implement quadruped robot simulation in gazebo and ROS2 environments. At the same time, a Riz2-based visualization tool is provided to forward the robot's status lcm data to ROS2 topics.

 For detailed information, please refer to [**Simulation Platform Document**](https://miroboticslab.github.io/blogs/#/cn/cyberdog_gazebo_cn)

 Recommended installation environment: Ubuntu 20.04 + ROS2 Galactic

 ## Dependency installation
 To run the simulation platform, you need to install the following dependencies:
 **ros2_Galactic**
 ```
 $ sudo apt update && sudo apt install curl gnupg lsb-release
 $ sudo curl -sSL https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -o /usr/share/keyrings/ros-archive-keyring.gpg
 $ echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/ros-archive-keyring.gpg] http://packages.ros.org/ros2/ubuntu $(  source /etc/os-release && echo $UBUNTU_CODENAME) main" | sudo tee /etc/apt/sources.list.d/ros2.list > /dev/null
 $ sudo apt update
 $ sudo apt install ros-galactic-desktop
 ```
 **Gazebo**
 ```
 $ curl -sSL http://get.gazebosim.org | sh
 $ sudo apt install ros-galactic-gazebo-ros
 $ sudo apt install ros-galactic-gazebo-msgs
 ```
 **LCM**
 ```
 $ git clone https://github.com/lcm-proj/lcm.git
 $ cd lcm
 $ mkdir build
 $ cd build
 $ cmake -DLCM_ENABLE_JAVA=ON ..
 $ make
 $ sudo make install
 ```
 **Eigen**
 ```
 $ git clone https://github.com/eigenteam/eigen-git-mirror
 $ cd eigen-git-mirror
 $ mkdir build
 $ cd build
 $ cmake ..
 $ sudo make install
 ```
 **xacro**
 ```
 $ sudo apt install ros-galactic-xacro
 ```

 **vcstool**
 ```
 $ sudo apt install python3-vcstool
 ```

 **colcon**
 ```
 $ sudo apt install python3-colcon-common-extensions
 ```

 Note: If there are other versions of yaml-cpp installed in the environment, it may conflict with the yaml-cpp that comes with ros galactic. It is recommended that there be no other versions of yaml-cpp in the compilation environment.

 ## download
 ```
 $ git clone https://github.com/MiRoboticsLab/cyberdog_sim.git
 $ cd cyberdog_sim
 $ vcs import < cyberdog_sim.repos
 ```
 ## Compile
 Need to set BUILD_ROS in src/cyberdog locomotion/CMakeLists.txt to ON
 Need to compile in the cyberdog_sim folder
 ```
 $ source /opt/ros/galactic/setup.bash
 $ colcon build --merge-install --symlink-install --packages-up-to cyberdog_locomotion cyberdog_simulator
 ```

 ## use
 Need to run under the cyberdog_sim folder
 ```
 $ python3 src/cyberdog_simulator/cyberdog_gazebo/script/launchsim.py
 ```

 ### You can also run each program separately through the following commands:

 First start the gazebo program and perform the following operations in the cyberdog_sim folder:
 ```
 $ source /opt/ros/galactic/setup.bash
 $ source install/setup.bash
 $ ros2 launch cyberdog_gazebo gazebo.launch.py
 ```
 You can also open lidar through the following command
 ```
 $ source /opt/ros/galactic/setup.bash
 $ source install/setup.bash
 $ ros2 launch cyberdog_gazebo gazebo.launch.py ​​use_lidar:=true
 ```

 Then start the cyberdog_locomotion control program.  Open a new terminal in the cyberdog_sim folder
 ```
 $ source /opt/ros/galactic/setup.bash
 $ source install/setup.bash
 $ ros2 launch cyberdog_gazebo cyberdog_control_launch.py
 ```

 Finally, open the visual interface and perform the following operations in the cyberdog_sim folder:
 ```
 $ source /opt/ros/galactic/setup.bash
 $ source install/setup.bash
 $ ros2 launch cyberdog_visual cyberdog_visual.launch.py
 ```

 ### Play real machine lcm log data
 The platform can play and visualize the motion control lcm data of the real machine through rviz2.
 Here's how to do it:
 Run the lcm data visualization interface and perform the following operations in the cyberdog_sim folder:
 ```
 $ source /opt/ros/galactic/setup.bash
 $ source install/setup.bash
 $ ros2 launch cyberdog_visual cyberdog_lcm_repaly.launch.py
 ```
 After opening the visual interface, play the lcm log data by running the script in the script directory of the cyberdog_locomotion warehouse.
 Perform the following operations in the cyberdog_sim folder:
 ```
 $ cd src/cyberdog_locomotion/scripts
 $ ./make_types.sh #Need to run when using for the first time
 $ ./launch_lcm_logplayer.sh
 ```
 After running, select the lcm log file that needs to be played to play the log data. At this time, the robot's posture can be reproduced through the rviz visualization interface.
