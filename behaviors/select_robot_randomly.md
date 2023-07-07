# Mission planning and assignment

Behavior

Forever ofthen the coordinator receives a mission requests.
For each mission request:
- the coordinator eventually create a mission plan.
- the coordinator eventually assigns a set of robot to fulfill a role in the mission plan.

Datatypes
 - MissionRequest
 - MissionPlan
 - MissionAssignment

Invariant
 - Eventually the coordinator should create a mission plan.
 - Eventually the coordinator should assign a robot to a role within the plan

Extends
 - Channels

Uncertainty / Obstacle
 - A role may not be assignable to any available robot.
 - A better robot may become available after a role has been assigned.
 - A robot may not be able to complete an assignment.
 - A mission may be cancelled. 