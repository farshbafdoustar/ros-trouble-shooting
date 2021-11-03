# ROS Trouble Shooting
Tip and Tricks for trouble shooting ros common packages in action

## Joint Trajectory Controller
### Trajectory Start Time
If the goal.trajectory.header.stamp is set when the trajectory is built, if there is a time delay till the trajectory goal reaches the cntroller, the start time will be behind the current time of controller nad the controller skips the first trajectory points that are happening in the past. That could lead to a big jump at the begining of the motion. One way is adding delay or keeping this time empty (zero). When the time is zero the trajectory will begin when the goal is reached the controller.
```
trajectory_.header.stamp = ros::Time();
```

as described in [Trajectory replacement](http://wiki.ros.org/joint_trajectory_controller):

> Joint trajectory messages allow to specify the time at which a new trajectory should start executing by means of the header timestamp, where zero time (the default) means "start now". 

### Trajectory Constraints
joint trajectory controller goal has default value 0 for all goal and path constraints except **stopped_velocity_tolerance**
> constraints/stopped_velocity_tolerance (double, default: 0.01) 

these could lead to trajectory abort with error code _GOAL_TOLERANCE_VIOLATED=-5_

best way is defining these constraints based on mechanical settings in real or simulation environemnt. but to overcome this trajectory abort in simulation testing need to set this to zero.

