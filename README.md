
![example workflow](https://github.com/TIERS/ros-dwm1001-uwb-localization/actions/workflows/main.yml/badge.svg)

# ROS Interface Node for DWM1001 Tags

This repo simplifies and extends the interfaces provided by `https://github.com/20chix/dwm1001_ros.git`, with nodes for both active and passive UWB tags. The code has been tested under Ubuntu 16.04/ROS Kinetic and Ubuntu 18.04/ROS Melodic only.

## Installation

This instructions are for Ubuntu 18.04 with ROS Melodic already installed.

Clone this repo into your catkin_ws (the code below creates a new catkin workspace named `uwb_ws` in your home folder):
```
mkdir -p  ~/uwb_ws/src && cd ~/uwb_ws/src
git clone https://github.com/TIERS/ros-dwm1001-uwb-localization.git
```

and install dependencies
```
sudo apt install python-dev python-pip
sudo -H python -m pip install --upgrade pip
sudo -H python -m pip install pyserial
```

Then build the workspace. We recommend using `catkin build`. Install it if needed with
```
sudo apt install python-catkin-tools
```

then run
```
cd ~/uwb_ws
catkin init
catkin build
```

## Get Started

Follow the steps in [Decawave's DRTLS Guide](https://www.decawave.com/wp-content/uploads/2018/08/mdek1001_quick_start_guide.pdf) to setup a DRTLS system with at least 3 anchors and 1 active tag. 

We recommend setting the tag's position update rate to 10 Hz in stationary mode too.

## Scripts

This repository contains ROS nodes written in Python to interface with the two types of UWB Tags running Decawave's default RTLS firmware. The nodes communicate with the tags throught Decawave's UART API.

- The node `dwm1001_active.py` obtains position information of all the anchors in the DRTLS system within range, the distances to them and the tag position. 

- The node `dwm1001_passive.py` gives the positions of all other active tags in the DRTLS system.

The active tags publishes anchor positions to `/dwm1001/anchor/AN*/position`, and its own position and distances to anchors to `/dwm1001/tag/tag_name/position` and `/dwm1001/tag/tag_name/to/anchor/AN*/distance`, respectively. The __tag_name_ is given as a parameter.

## Run

Then simply run for an active tag (after checking which port it is connected to, usually starting from `/dev/ttyACM0`):

```
source ~/uwb_ws/devel/setup.bash
rosrun dwm1001_uwb_tag_drivers dwm1001_active _port:=/dev/ttyACMX/ _tag_name:="your_tag_name"
``` 

or for a passive tag:

```
rosrun dwm1001_uwb_tag_drivers dwm1001_passive _port:=/dev/ttyACMX/
```

with the corresponding port where the UWB tag is connected. The `tag_name` is only used for the topic names.
