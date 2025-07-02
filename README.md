ROS 2 CPU Scheduler — FCFS / Round Robin / Priority

This project simulates a CPU scheduling system using ROS 2 (Humble) and Python (rclpy). It supports three widely-used scheduling algorithms:

- FCFS – First-Come, First-Served
- RR – Round Robin
- PRIORITY – Preemptive Priority Scheduling

Two core ROS 2 nodes are implemented:

- SchedulerNode.py – Coordinates process execution and applies the selected scheduling algorithm
- ProcessNode.py – Simulates a user process that requests CPU time from the scheduler

The system uses ROS 2 topics to communicate between the scheduler and the processes. A **double handshaking protocol** ensures reliable communication:
1. Each ProcessNode repeatedly sends a REQUEST-message(as a Thread) until it receives an ACK from the SchedulerNode.
2. Once acknowledged, the SchedulerNode controls the process by sending START, PAUSE, and FINISHED messages, depending on scheduling logic.

------------------------------------------------------------
How to Run

1) Requirements

- ROS 2 Humble
- Python 3.8+
- rclpy, std_msgs

------------------------------------------------------------
2) Build and Source the Workspace

unzip the CPU_ROS.rar then:
<pre>
cd ~/ros2_ws
colcon build
source install/setup.bash
</pre>

------------------------------------------------------------
3) Launch the Scheduler Node
<pre>
ros2 run CPU_ROS SchedulerNode --type FCFS
</pre>
You can replace FCFS with RR or PRIORITY to use a different scheduling algorithm.

------------------------------------------------------------
4) Launch Process Nodes (each in a new terminal)
<pre>
ros2 run CPU_ROS ProcessNode Proc_1 13 --priority 1
ros2 run CPU_ROS ProcessNode Proc_2 7 --priority 2
ros2 run CPU_ROS ProcessNode Proc_3 5 --priority 3
</pre>
------------------------------------------------------------
Features

- ROS 2 topic-based message passing between nodes
- Simulated time slicing for Round Robin
- Preemption in Priority Scheduling mode
- Modular, thread-safe architecture using rclpy
- Double handshaking between processes and scheduler

------------------------------------------------------------
Example Output

SchedulerNode:
<pre>
moath@MOATH:~/ros2_ws$ ros2 run CPU_ROS SchedulerNode --type FCFS
Starting Scheduler Node with scheduling type: FCFS
[INFO] [1751453752.970312000] [scheduler_node]: 
[STATUS] CPU Idle
[STATUS] Queue: []
--------------------------------------------------
[INFO] [1751453758.921040800] [scheduler_node]: 
[STATUS] Time: 1s
[STATUS] CPU Executing: Proc_1 (Remaining: 13.0s)
[STATUS] Queue: []
--------------------------------------------------
[INFO] [1751453759.920584300] [scheduler_node]: 
[STATUS] Time: 2s
[STATUS] CPU Executing: Proc_1 (Remaining: 12.0s)
[STATUS] Queue: []
--------------------------------------------------
[INFO] [1751453760.934850200] [scheduler_node]: 
[STATUS] Time: 3s
[STATUS] CPU Executing: Proc_1 (Remaining: 11.0s)
[STATUS] Queue: []
--------------------------------------------------
[INFO] [1751453761.948745500] [scheduler_node]: 
[STATUS] Time: 4s
[STATUS] CPU Executing: Proc_1 (Remaining: 10.0s)
[STATUS] Queue: []
--------------------------------------------------
[INFO] [1751453762.924334400] [scheduler_node]: 
[STATUS] Time: 5s
[STATUS] CPU Executing: Proc_1 (Remaining: 9.0s)
[STATUS] Queue: []
--------------------------------------------------
[INFO] [1751453763.940636600] [scheduler_node]: 
[STATUS] Time: 6s
[STATUS] CPU Executing: Proc_1 (Remaining: 8.0s)
[STATUS] Queue: []
--------------------------------------------------
[INFO] [1751453764.932991900] [scheduler_node]: 
[STATUS] Time: 7s
[STATUS] CPU Executing: Proc_1 (Remaining: 7.0s)
[STATUS] Queue: []
--------------------------------------------------
[INFO] [1751453765.922171400] [scheduler_node]: 
[STATUS] Time: 8s
[STATUS] CPU Executing: Proc_1 (Remaining: 6.0s)
[STATUS] Queue: []
--------------------------------------------------
[INFO] [1751453766.929632700] [scheduler_node]: 
[STATUS] Time: 9s
[STATUS] CPU Executing: Proc_1 (Remaining: 5.0s)
[STATUS] Queue: []
--------------------------------------------------
[INFO] [1751453767.921141000] [scheduler_node]: 
[STATUS] Time: 10s
[STATUS] CPU Executing: Proc_1 (Remaining: 4.0s)
[STATUS] Queue: []
--------------------------------------------------
[INFO] [1751453768.937561200] [scheduler_node]: 
[STATUS] Time: 11s
[STATUS] CPU Executing: Proc_1 (Remaining: 3.0s)
[STATUS] Queue: []
--------------------------------------------------
[INFO] [1751453769.923282800] [scheduler_node]: 
[STATUS] Time: 12s
[STATUS] CPU Executing: Proc_1 (Remaining: 2.0s)
[STATUS] Queue: []
--------------------------------------------------
[INFO] [1751453770.941080200] [scheduler_node]: 
[STATUS] Time: 13s
[STATUS] CPU Executing: Proc_1 (Remaining: 1.0s)
[STATUS] Queue: []
--------------------------------------------------
[INFO] [1751453771.922865800] [scheduler_node]: Process Proc_1 finished execution.
[INFO] [1751453771.924730300] [scheduler_node]: 
[STATUS] CPU Idle
[STATUS] Queue: []
--------------------------------------------------
</pre>

ProcessNode:
<pre>
moath@MOATH:~/ros2_ws$ ros2 run CPU_ROS ProcessNode Proc_1 13 --priority 1
[INFO] [1751453758.668531900] [process_Proc_1]: Sending process request for Proc_1
[INFO] [1751453758.673219900] [process_Proc_1]: Received ACK for Proc_1
[INFO] [1751453758.921185500] [process_Proc_1]: Process Proc_1 started with time slice 1s. Remaining=13.0s
[INFO] [1751453771.923339100] [process_Proc_1]: Received FINISHED for Proc_1. Shutting down.
moath@MOATH:~/ros2_ws$ 
</pre>
