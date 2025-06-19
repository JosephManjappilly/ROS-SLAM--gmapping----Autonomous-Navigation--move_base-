# ROS SLAM (gmapping) - Autonomous Navigation (move_base)

This repository contains a robotics project focused on implementing Simultaneous Localization and Mapping (SLAM) and autonomous navigation for a custom robot using ROS (Robot Operating System). The project leverages key components of the ROS Navigation Stack, with a particular focus on `gmapping` for SLAM and `move_base` for autonomous navigation.


---

## 📚 Project Overview

This project is primarily based on the concepts and structure introduced in the Udemy course:
**[ROS SLAM Navigation Stack and Custom Robot](https://github.com/noshluk2/ROS-Navigation-Stack-and-SLAM-for-Autonomous-Custom-Robot/tree/master)**

The project is divided into two main sections:

1.  **Implementation of SLAM with `gmapping` (Mapping Phase)**
2.  **Autonomous Navigation with `move_base` (Navigation Phase)**

---

## 1. 🗺 SLAM Implementation (`map.launch`)

The first part of the project focuses on achieving robust SLAM using the `slam_gmapping` node. The solution for this phase is encapsulated in the `map.launch` file.

**Key enhancements and changes from the Udemy course implementation:**

* **Improved Robot State Publishing:** Significant modifications were made to the robot state publishing node. Additional parameters such as `wheel_radius`, `wheel_separation`, and specifically defined `left_wheel_joint` and `right_wheel_joint` were incorporated as per the robot's URDF specifications.
* **Enhanced Mapping Accuracy:** By integrating these detailed robot kinematics into the state publishing, a notable increase in mapping accuracy was observed. This reduced errors in the robot's understanding of the generated map, leading to more reliable SLAM performance.

**To run the SLAM mapping phase:**

```bash
roslaunch explorer_bot map.launch
````
## 2. 🧭 Autonomous Navigation (`navigation.launch`)

The second part of the project combines SLAM with autonomous navigation, allowing the robot to map and navigate simultaneously within an unknown environment. The complete solution for this is found in the `navigation.launch` file.

**Key configuration adjustments and fixes compared to the Udemy course:**

* **TEB Local Planner Loading:** The `TebLocalPlannerROS` was initially not loading correctly due to incorrect namespace specification. This was resolved by explicitly defining the namespace for the TEB local planner directly within the `navigation.launch` file.
* **Correct Parameter Loading with Namespacing:** A crucial fix involved addressing nested namespace issues. It was identified that specifying a namespace in the launch file (e.g., `ns="global_costmap"`) while simultaneously having a top-level namespace key within the `.yaml` configuration files (e.g., `global_costmap:`) led to improper parameter loading (e.g., `/navigation/global_costmap/global_costmap/param`). To fix this, all top-level namespace keys were removed from the `costmap_common_params.yaml`, `local_costmap_params.yaml`, `global_costmap_params.yaml`, and `trajectory_planner.yaml` configuration files, ensuring that namespaces are *only* defined in the launch file. This guarantees correct parameter hierarchy and loading.

**To run the autonomous navigation phase:**

```bash
roslaunch explorer_bot navigation.launch
```

