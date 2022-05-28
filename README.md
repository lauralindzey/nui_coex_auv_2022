# CoExploration on NUI

The CoExploration utilities were first demonstrated on NUI during the 2022 OECI cruise on E/V Nautilus.

This repo contains launch files to run multiresolution map transfer and progressive image transfer using logged data from NUI's dive 040.


## Install

For the ROS parts:
* `mkdir -p ~/coex_ws/src`
* `cd ~/coex_ws/src`
* download `coex.repos`
* `vcs import --input coex.repos`
  - Assumes you have python3-vcstool installed
  - ros_acomms asks for a password, which doesn't play nicely with `vcs import`, so it may be necessary to do this manually: `git clone https://git.whoi.edu/acomms/ros_acomms.git`
* `cd ~/coex_ws`; `rosdep install --from-paths src --ignore-src -r -y`
* `source /opt/ros/noetic/setup.bash`
* Follow instructions in the progressive_imagery README to install its dependencies
* `catkin build`   
  - Assumes you have python3-catkin-tools installed


## Launching Processes

Before starting the processes, be sure that the logging directories are set up and empty. By default, they are:
  - /data/coex/rx_grids
  - /data/coex/pi_data/rx
  - /data/coex/pi_data/tx

If you prefer to put them somewhere else, use the `log_path:=` argument when launching demo_coex.launch.

Then, open 3 terminal windows (in each, run `source ~/coex_ws/devel/setup.bash`):
* `roscore`
* `rosparam set use_sim_time true`; `roslaunch nui_coex_auv_2022 demo_coex.launch`
* `rosbag play --clock ~/coex_ws/src/nui_coex_auv_2022/data/nui040_survey.bag`

The map will begin streaming automatically.

In order to stage an image for transmission, copy it to /data/coex/pi_data/tx. e.g.: `cp ~/coex_ws/src/nui_coex_auv_2022/data/image_001784.tiff /data/coex/pi_data/tx/`


To look at the raw pointclouds from the Norbit:
`rosrun rviz rviz -d ~/coex_ws/src/nui_coex_auv_2022/nui.rviz`

## User Interfaces
Launching the above ROS nodes will cause 3 different GUIs to pop up.

1) Message Queue Node Configuration GUI

Use this to enable/disable queues, as well as adjust their priorities. Any change of state emits a QueuePriority or QueueEnable message, which is then forwarded via ros_acomms to the remote modem.

2) Quadtree Grid GUI

The subsea node will automatically stream grids as space allows in the ros_acomms packets.

In order to request data:
* Click "Select region active"
* drag box around area of interest
* use radio buttons to select desired resolution
* Click "Send Selection"

3) Progressive Imagery

Images staged for transport will show up in the lower-left "Sender Stats" box; adjust priorities using the lower-right "New Goals" box. Each image ID may have up to one "goal" percentage specified by the scientist. Fill in ID and goal, click "Add". When priorities are as desired, click "Publish".
