# ROS Navigation stack in Gazebo with Agilex Scout Mini

## 1. Additional Custom Global Planner Plugin

### Download Global Planner Plugin

* [rrtx global planner](https://github.com/kangbeenlee/rrtx_global_planner.git)
* [ompl global planner](https://github.com/kangbeenlee/ompl_global_planner.git)

### How to use

* Create your own workspace (description in 2.1).
* Download above plugin in (your workspace)/src directory.

## 2. Installation and Navigation

### 0. Install the Gazebo software

* Gazebo is a robot simulator. Gazebo simulates multiple robots in a 3D environments, with extensive dynamic interaction between objects.
* Gazebo Download Link : [http://gazebosim.org](http://gazebosim.org/)
* Download and install gazebo. You can go to the website : http://gazebosim.org/install

### 1. Installation

1. **Development Environment ubuntu 20.04 + [ROS Noetic desktop full](http://wiki.ros.org/noetic/Installation/Ubuntu)**

2. **Build ROS packages for Scout simulator**
    
    Create worksapce, download ROS packages

    ```
    mkdir -p ~/scout_ws/src
    cd ~/scout_ws/src
    git clone https://github.com/kangbeenlee/gazebo_navigation.git
    ```

3.  **Install required ROS packages**
    
    Noetic (ROS version)

    ```
    cd gazebo-navigation
    sh install_dependencies_noetic.sh
    ```

4. **Install dependencies and build**

    ```
    cd ~/scout_ws
    rosdep install --from-paths src --ignore-src -r -y
    catkin_make
    ```

### 2. Simple Test

1. **SCOUT-MINI description**

    ```
    cd ~/scout_ws
    source devel/setup.bash
    roslaunch scout_description display_scout_mini.launch 
    ```

2. **Launch gazebo simulator and teleop control**
    
    1. Launch gazebo simulator

        ```
        cd ~/scout_ws
        source devel/setup.bash
        roslaunch scout_gazebo_sim scout_mini_playpen.launch
        ```

    2. Run teleop controller (w, a, s, d)
        
        ```
        # Open another terminal
        cd ~/scout_ws
        roslaunch scout_teleop scout_teleop_key.launch 
        ```

### 3. 2D Navigation

1. **Gmapping SLAM mapping**
    ```
    # Run gazebo simulator
    roslaunch scout_gazebo_sim scout_mini_playpen.launch
    ```

    ```
    # Run ekf_filter.launch to make odom frame
    roslaunch scout_localization ekf_filter.launch
    ```

    ```
    # Run gmapping slam
    roslaunch scout_slam scout_slam.launch
    ```

    ```
    # Save map (in another terminal)
    roslaunch scout_slam gmapping_save.launch
    ```

2. **Navigation**

    * EKF is applied to sensor fuse encoder and imu for odometry
    
    1. **Run navigation with default global planner**

        ```
        roslaunch scout_navigation scout_navigation.launch
        ```

    2. **Run navigation with rrtx global planner**

        ```
        roslaunch scout_navigation scout_navigation.launch base_global_planner:=rrtx_global_planner/RRTXPlanner rviz_option:=rrtx
        ```
    
    3. **Run navigation with ompl global planner**

        ```
        roslaunch scout_navigation scout_navigation.launch base_global_planner:=ompl_global_planner/OmplGlobalPlanner rviz_option:=ompl
        ```