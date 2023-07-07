# Robot Execute a Mission

The robot executes tasks within a mission.
Periodically the robot update the status of an ongoing task to the coordinator.
When all tasks are completed the robot report mission completed. 
When the robot complete the tasks it enters the state `concluding mission`.
When coordinator and robot agrees on the mission completion the robot changes state.


# Invariant
- After mission completed, eventually the coordinator should register mission completed.
