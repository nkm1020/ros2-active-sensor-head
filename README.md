# ROS 2 Active Sensor Head

A project-first robotics portfolio: a two-degree-of-freedom pan-tilt sensor head that actively observes unstructured terrain, implemented in ROS 2 and evaluated against a fixed-sensor baseline.

> Status: planning and first implementation

## Why this project

Mobile robots operating in unstructured construction or exploration environments can suffer from blind spots when their sensors remain fixed. This project investigates whether an actively controlled sensor head can improve environmental coverage and obstacle discovery without requiring a new mobile robot platform.

The project is inspired by space and remote-construction robotics, but it does **not** claim flight readiness or lunar-environment validation.

## Final objective

Build and evaluate a reproducible system that:

1. models a 2-DOF pan-tilt sensor head in CAD and URDF/Xacro;
2. commands the head through ROS 2;
3. simulates the mechanism and an RGB-D camera or LiDAR in Gazebo;
4. performs configurable scanning over uneven, partially occluded terrain;
5. records sensor and joint data;
6. compares active scanning with a fixed-forward sensor using quantitative metrics.

## Minimum viable system

- Python scan planner producing pan and tilt targets
- ROS 2 command and state topics
- URDF/Xacro model with valid TF frames, limits, mass, and inertia
- RViz visualization
- Gazebo simulation with ros2_control
- RGB-D camera or LiDAR integration
- Fixed-sensor baseline and active-scanning method
- Repeatable experiment runner and CSV results
- Plots, demo video, and English technical report

## Stretch goals

Only after the minimum system is complete:

- C++ implementation of one ROS 2 node
- obstacle-directed or coverage-aware scan planning
- RTAB-Map integration
- 3D-printed prototype with two servos
- MATLAB cross-check of kinematics
- tests under low-texture or low-light simulated conditions

## Non-goals

- developing SLAM from scratch
- training a large deep-learning model
- full humanoid or legged-robot control
- high-fidelity lunar physics
- reinforcement learning before the baseline system is complete
- claiming real-world space qualification

## Planned architecture

~~~text
scan_planner
    -> /head/target_joint_states
head_controller
    -> simulated pan/tilt joints
    -> /joint_states

RGB-D camera or LiDAR
    -> /camera/* or /points
    -> coverage_evaluator
    -> experiment CSV and plots

robot_description
    -> URDF/Xacro
    -> TF tree
    -> RViz and Gazebo
~~~

## Development milestones

### M0: Numerical scan prototype
Generate configurable pan/tilt trajectories in Python and plot target angle, velocity, and time.

### M1: ROS 2 message flow
Separate scan-planner and head-controller nodes exchange targets and states through ROS 2 topics.

### M2: Visual robot model
A primitive-box URDF moves correctly in RViz with a valid TF tree and joint limits.

### M3: Mechanical model
Replace primitive geometry with an original CAD design and documented mass/inertia assumptions.

### M4: Physics simulation
Control the head in Gazebo through ros2_control and measure joint tracking error.

### M5: Sensor integration
Attach an RGB-D camera or LiDAR, record data, and visualize the observed environment.

### M6: Controlled experiment
Compare a fixed-forward baseline with active scanning in at least three scenes and repeated runs.

### M7: Portfolio release
Publish reproducible instructions, results, limitations, a demo video, and a short English report.

See [docs/ROADMAP.md](docs/ROADMAP.md) for milestone gates and just-in-time learning topics.

## Evaluation

The final experiment will report at least:

- observed-surface or voxel coverage rate
- obstacle discovery time
- blind-spot rate
- mean and maximum joint tracking error
- processing rate or latency
- total scan duration

Every comparison must use the same world, initial pose, sensor configuration, and time budget. Repeated trials and failures will be retained rather than showing only the best run.

## Definition of done

The project is complete when a new user can follow the README on a clean supported environment, run both baseline and active experiments, reproduce the result tables, and understand the main limitations.

A visually convincing simulation alone is not considered complete.

## Target software stack

Initial target, subject to an environment check:

- Ubuntu 24.04
- ROS 2 Jazzy
- Gazebo Harmonic
- Python 3
- C++ for one later node
- RViz, URDF/Xacro, TF2, ros2_control
- Git and GitHub

## Project-first learning rule

No tool is studied in isolation. Each new concept is learned only when the next working feature requires it:

1. define one observable behavior;
2. attempt the implementation;
3. identify the exact knowledge gap;
4. learn the minimum concept needed;
5. apply it immediately;
6. test, document, and commit;
7. move to the next behavior.

All committed code must be explainable and modifiable by the project owner.

## Planned repository structure

~~~text
src/
  scan_planner/
  head_control/
  head_description/
  perception/
  evaluation/
simulation/
  worlds/
hardware/
  cad/
experiments/
docs/
~~~

## Current next step

Complete M0: generate and plot the first configurable pan-tilt scan trajectory in Python.

## License

No open-source license has been selected while the repository is private. A license will be chosen before public release.
