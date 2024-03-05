# RC state publisher mod for Bunker ROS packages
by E. Aaltonen March 2023

This project includes additions and modifications necessary to publish RC data in Agilex Bunker (https://github.com/agilexrobotics/bunker_ros (license: BSD)). 

## New files:

* bunker_msgs/msg/BunkerRCState.msg

## Modified files:

* bunker_msgs/CMakeList.txt
* bunker_base/include/bunker_base/bunker_messenger.hpp
* bunker_base/src/bunker_messenger.cpp

RC data is published in topic /bunker_rc_state:

```
uint8 swa
uint8 swb
uint8 swc
uint8 swd
int8 stick_right_v
int8 stick_right_h
int8 stick_left_v
int8 stick_left_h
int8 var_a
```

## Changes:
```
diff bunker_base/include/bunker_base/bunker_messenger.hpp.original bunker_base/include/bunker_base/bunker_messenger.hpp:


8a9
>   // Modifications by EA Mar 5, 2024: added rc_publisher_ 
54a56
>     ros::Publisher rc_publisher_;   // EA    


diff bunker_base/src/bunker_messenger.cpp.original bunker_base/src/bunker_messenger.cpp:


8a9
>  // Modifications by EA Mar 5, 2024: added RC state publisher (rc_publisher_)
16c17
< 
---
> #include "bunker_msgs/BunkerRCState.h" // EA
31c32,33
< 
---
>     rc_publisher_ = nh_->advertise<bunker_msgs::BunkerRCState>("/bunker_rc_status", 10);   // EA
>     
111a114,129
>     
>     // publish Bunker RC state message // EA ->
>     bunker_msgs::BunkerRCState rc_msg;
>     
>     rc_msg.swa = robot_state.rc_state.swa;
>     rc_msg.swb = robot_state.rc_state.swb;
>     rc_msg.swc = robot_state.rc_state.swc;
>     rc_msg.swd = robot_state.rc_state.swd;
>     rc_msg.stick_right_v = robot_state.rc_state.stick_right_v;
>     rc_msg.stick_right_h = robot_state.rc_state.stick_right_h;
>     rc_msg.stick_left_v = robot_state.rc_state.stick_left_v;
>     rc_msg.stick_left_h = robot_state.rc_state.stick_left_h;
>     rc_msg.var_a = robot_state.rc_state.var_a;
>     
>     rc_publisher_.publish(rc_msg); // -> EA
>     
199a218
> 


diff bunker_msgs/CMakeLists.txt.original bunker_msgs/CMakeLists.txt:


1c1
< 
---
> ## Modifications by E. Aaltonen 2024 // EA
52c52
< 
---
>     BunkerRCState.msg   #// EA
201a202
> 
```
