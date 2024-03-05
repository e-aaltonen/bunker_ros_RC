# RC state publisher mod for Bunker ROS packages
by E. Aaltonen March 2023

This project includes additions and modifications necessary to publish RC data in Agilex Bunker (https://github.com/agilexrobotics/bunker_ros (license: BSD) - branch: v1.x). 

## New files:

* bunker_msgs/msg/BunkerRCState.msg

## Modified files:

* bunker_msgs/CMakeList.txt
* bunker_base/include/bunker_base/bunker_messenger.hpp
* bunker_base/src/bunker_messenger.cpp

RC data is published in topic /bunker_rc_state:

```
uint8 sws
int8 var_a
int8_t right_stick_left_right 
int8_t right_stick_up_down
int8_t left_stick_left_right
int8_t left_stick_up_down

```

## Changes:
```
~/catkin_ws/src/base_ros/bunker_base/bunker_msgs/CMakeLists.txt:
53a54
>     BunkerRCState.msg   #//.

--
~/catkin_ws/src/base_ros/bunker_base/bunker_msgs/msg/BunkerRCState.msg:
uint8 sws
uint8 var_a

--

~/catkin_ws/src/base_ros/bunker_base/bunker_base/src/bunker_messanger.cpp
14a15
> #include "bunker_msgs/BunkerRCState.h"  //.
31a33,34
>     rc_publisher_ =
>         nh_->advertise<bunker_msgs::BunkerRCState>("/bunker_rc_status", 10); //.
207a211,218
>     // publish Bunker RC state message  //.
>     bunker_msgs::BunkerRCState rc_msg;
> 
>     rc_msg.sws = state.rc_state.sws;
>     rc_msg.var_a = state.rc_state.var_a;
>        
>     rc_publisher_.publish(rc_msg);
>         

--

~/catkin_ws/src/base_ros/bunker_base/bunker_base/include/bunker_base/bunker_messenger.hpp:


56a57
>     ros::Publisher rc_publisher_;       //.

--
```
