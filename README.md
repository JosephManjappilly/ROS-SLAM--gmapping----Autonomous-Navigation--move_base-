# 🧭 ROS SLAM (gmapping) - Autonomous Navigation (move_base)
## Custom Robot · Navigation Stack · TEB Planner · Experimental Evaluation

This repository implements a complete SLAM (Simultaneous Localization and Mapping) and Autonomous Navigation system for a custom differential-drive robot using ROS (Robot Operating System).

It extends the concepts from the Udemy course: 🔗 **[ROS SLAM Navigation Stack and Custom Robot](https://github.com/noshluk2/ROS-Navigation-Stack-and-SLAM-for-Autonomous-Custom-Robot/tree/master)**

The project now includes:

* A refined custom URDF robot

* SLAM using gmapping

* Autonomous navigation using move_base

* Integration of the TEB local planner

* Four experimental navigation configurations (Case 1–4)

Performance evaluation from 80 mapping trials
---


## 📌 Project Structure

This repository is organized into:

* **SLAM (Mapping) — Running gmapping**

* **Autonomous Navigation — Using move_base**

* Four experimental evaluation cases

* Updated costmap and planner configurations
---

## 🗺 1. SLAM Implementation (map.launch)

SLAM is implemented via the slam_gmapping package.

✔ Key Enhancements (Beyond the Base Udemy Project)

Added custom parameters to robot_state_publisher:
``` 
<param name="wheel_radius" value="0.035"/>
<param name="wheel_separation" value="0.25"/>
<param name="left_wheel_joint" value="left_wheel_joint"/>
<param name="right_wheel_joint" value="right_wheel_joint"/>
```
Although these parameters are not standard for the node, they were intentionally injected to analyze their effect on mapping performance.

Observed improvements:

* Higher mapping consistency
* Reduced variation across runs
### ▶ Run SLAM Mapping

```{bash}:Run SLAM Mapping:
roslaunch explorer_bot map.launch
```

## 🧭 2. Autonomous Navigation (`navigation.launch`)

Navigation combines SLAM, costmaps, and planning in the `move_base` framework.

### ✔ Major Fixes & Improvements

**1. TEB Local Planner Correctly Enabled**
`TebLocalPlannerROS` originally failed to load due to namespace issues.
This was fixed by:
* Adding the correct TEB namespace in `navigation.launch`
* Ensuring parameter YAML files do not include their own namespace blocks

**2. Costmap Namespace Fix**
Original YAML files contained:

```{yaml}:Original YAML Costmap Namespace:
global_costmap:
```

while the launch file already used:

```{xml}:Launch File Namespace:
<node ns="global_costmap">
```

This resulted in invalid nested paths like: `/navigation/global_costmap/global_costmap/param`.
**✔ Fixed by removing all top-level namespace keys from the costmap YAML files.**

### ▶ Run Autonomous Navigation

```{bash}:Run Autonomous Navigation:
roslaunch explorer_bot navigation.launch
```

## 🧪 3. Experimental Evaluation — 4 Cases

To analyze the impact of wheel geometry (injected parameters) and planner choice, four launch configurations were created:

| Case | Wheel Params | Local Planner | Launch File |
| :--- | :--- | :--- | :--- |
| **1** | ✘ Default | Default | `navigation_NWD_NT.launch` |
| **2** | ✔ Injected | Default | `navigation_WD_NT.launch` |
| **3** | ✘ Default | TEB | `navigation_NWD_WT.launch` |
| **4** | ✔ Injected | TEB | `navigation_WD_WT.launch` |

> *Each case was executed 20 times, resulting in 80 mapping runs. (Export to Sheets)*

## 📊 4. Performance Results Summary

### ✔ Mean Mapping Time (20 Trials Per Case)

| Case | Mean Time (sec) | Std Dev (sec) |
| :--- | :--- | :--- |
| C1: No WD + Default | 538.76 | 13.24 |
| C2: WD + Default | 487.66 | 5.80 |
| C3: No WD + TEB | 340.27 | 1.84 |
| C4: WD + TEB | 339.94 | 1.56 |

> *Export to Sheets*

### 🟢 Interpretation

* Wheel geometry improves mapping performance by **~10%** under the default planner.
* TEB offers a **~36–37% faster mapping time**.
* Standard deviation dramatically decreases → more consistent behavior.
* TEB + wheel parameters (Case 4) shows the most stable performance.



## 🧠 5. Observed Behavioral Differences

| Behavior | C1 | C2 | C3 | C4 |
| :--- | :--- | :--- | :--- | :--- |
| Handles sharp curves | ✘ | ✘ | ✔ | ✔ |
| Reverse arc turning | ✘ | ✘ | ✔ | ✔ |
| Gets stuck at corners | Frequent | Less | None | None |
| Path smoothness | Low | Medium | High | High |

> *Export to Sheets*

TEB significantly improved maneuverability and reduced oscillatory behavior.

## 🛠 6. File Structure Overview

```markdown:File Structure:
explorer_bot/
│── launch/
│   ├── map.launch
│   ├── navigation.launch
│   ├── navigation_NWD_NT.launch
│   ├── navigation_WD_NT.launch
│   ├── navigation_NWD_WT.launch
│   ├── navigation_WD_WT.launch
│── config/
│   ├── costmap_common_params.yaml
│   ├── local_costmap_params.yaml
│   ├── global_costmap_params.yaml
│   ├── teb_local_planner.yaml
│   ├── trajectory_planner.yaml
│── urdf/
│   ├── explorer_bot.urdf
│── worlds/
│── maps/
│── scripts/
```

## 🧩 7. Tools Used

* ROS Noetic
* Gazebo 11
* RViz
* TEB Local Planner
* `slam_gmapping`
* Custom URDF robot

## 📘 8. References

* ROS Navigation Stack
* TEB Local Planner (C. Rösmann et al.)
* Gmapping SLAM
* Base repository by noshluk2

## 🎯 9. Summary

This repository now provides:
* A complete ROS SLAM + Navigation pipeline
* Corrected namespaces and planner configuration
* Four experimental navigation setups
* Analysis of mapping time improvements
* Behavioral comparisons across planners
* Insights into how non-standard wheel parameters affect navigation

Feel free to explore, clone, modify, and expand upon this project!
