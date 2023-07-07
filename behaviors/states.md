
# Types:
 - request
 - mission
 - robot
 - coordinator
 - user

# Uncertainties
 - Messages can be lost
 - Coordinator may have outdated information about a robot
  

# Initial state:

userA:
    request: none
    requests: []
    received_feedback: none

mission:
    status: none
    tasks: []

robotA: 
    status: none
    assignment: none
    coordination_channel: none

coordinator:
    pending_missions: []
    assigned_missions: []
    robots: []

# Actions:
## A user request mission

- user_generate_request
UserA.request: RequestA

- user_send_request
/\ send_request(UserA, UserA.RequestA, Coordinator)
 \/ append_pending_mission(Coordinator, RequestA)
 \/ reply_to_user(RequestA, 'busy') # too buy to handle the request

- coordinator_assign_mission
mission = pop_pending_mission(Coordinator)
 /\ plan = plan_mission(mission, Coordinator)
 /\ (update, error) = assign_mission(plan)
 /\ update_coordinator_knowledge(Coordinator, update)
 /\ append_assigned_mission(Coordinator, plan)
 
 \/ append_pending_mission(Coordinator, mission) # replan later
 \/ reply_to_user(mission, failure_reason)

    
- robot_execute_mission
\/ task = select_next_task(Robot)
    /\ execute_task(Robot, task)
\/ robot_report_mission_concluded(Robot)


- robot_execute_task
[has_selected_task(Robot)]
\/ progress_on_task(Robot)
    /\ update_coordinator_knowledge(Coordinator, progress_on_task)

\/ task_completed(Robot)
    /\ update_coordinator_knowledge(coordination_channel, task_completed)
    /\ robot_execute_mission(Robot)

\/ find_obstacle(Robot)
    /\ update_coordinator_knowledge(Coordinator, handle_obstacle)
\/ task_failed(Robot)


- robot_report_mission_concluded



- robot_discovery
[robot_available(Robot)]
lookup_coodinator(Robot)
<transaction>
/\ Robot.coordination_channel = channel(Coordinator, Robot)
/\  
/\ update_coordinator_knowledge(Coordinator, Robot)
/\ update_robot_knowledge(Robot, Coordinator)


