# Project-First Roadmap

This roadmap is organized by working behavior, not by courses or calendar weeks. Do not study a topic because it appears later in the roadmap. Learn it only when the current milestone requires it.

## Working cycle

For every milestone:

1. define the next observable behavior;
2. make the smallest implementation attempt;
3. write down the exact blocker;
4. learn only the concept needed to remove that blocker;
5. apply it immediately;
6. test against the milestone gate;
7. record evidence and commit;
8. explain the result without reading generated code.

A milestone is complete only when its gate is satisfied. Time spent watching tutorials is not progress by itself.

## M0: Numerical scan prototype

### Observable behavior

A Python program generates a two-axis scan trajectory over time and plots pan angle, tilt angle, and angular velocity.

### Build tasks

- create configurable pan and tilt limits;
- generate a repeatable raster or sweeping pattern;
- keep all internal angles in radians;
- export trajectory data to CSV;
- plot angles and velocities;
- reject invalid limits or speeds.

### Learn only when needed

- running Python files;
- variables, functions, loops, and conditionals;
- lists and NumPy arrays;
- degree/radian conversion;
- plotting with Matplotlib;
- basic file input/output;
- reading tracebacks.

### Gate

- changing limits and speed does not require editing the algorithm;
- the generated trajectory stays inside all limits;
- output CSV and plots can be reproduced with one command;
- the owner can explain every function and modify the scan pattern.

### Evidence

- source code;
- one example CSV;
- one plot;
- short explanation in the commit or development log.

## M1: ROS 2 message flow

### Observable behavior

A scan-planner node publishes target pan/tilt angles and a separate controller node receives and reports them.

### Build tasks

- create ROS 2 Python packages;
- define topic names and message types;
- publish targets at a controlled rate;
- subscribe and validate incoming targets;
- expose limits and scan rate as parameters;
- launch both nodes together;
- record a short rosbag.

### Learn only when needed

- ROS 2 workspace and package structure;
- nodes, topics, publishers, subscribers;
- parameters, launch files, and timestamps;
- terminal commands and environment sourcing.

### Gate

- both nodes start from one launch command;
- topic data can be inspected from the terminal;
- parameters change behavior without source edits;
- invalid commands are rejected or clamped and reported.

## M2: Visual robot model

### Observable behavior

A primitive 2-DOF head moves in RViz according to the commanded joint states.

### Build tasks

- define base, pan, tilt, and sensor links;
- define two revolute joints and limits;
- publish robot state and joint state;
- inspect the TF tree;
- verify axes and sign conventions.

### Learn only when needed

- link and joint concepts;
- URDF/Xacro syntax;
- parent/child frames;
- roll, pitch, yaw;
- TF2 and JointState;
- basic rotation matrices and coordinate conventions.

### Gate

- no disconnected TF frames;
- positive pan and tilt rotate in the documented directions;
- visual motion respects limits;
- the model loads without URDF errors.

## M3: Mechanical CAD model

### Observable behavior

The primitive geometry is replaced by an original, mechanically plausible pan-tilt assembly.

### Build tasks

- select approximate sensor mass and dimensions;
- design base, brackets, shafts, and sensor mount;
- define clear rotation axes and assembly constraints;
- check interference through full motion;
- export meshes with correct units and origins;
- document estimated mass and inertia.

### Learn only when needed

- sketches, dimensions, and constraints;
- assemblies and revolute mates;
- center of mass and moment of inertia;
- fabrication clearances;
- mesh export and coordinate alignment.

### Gate

- no interference across the documented range;
- exported meshes align with URDF joints;
- dimensions, mass assumptions, and joint limits are recorded;
- the model remains visually and kinematically correct in RViz.

## M4: Physics simulation and control

### Observable behavior

Gazebo simulates the head and tracks commanded angles through ros2_control.

### Build tasks

- add collision and inertial properties;
- configure controllers;
- send trajectories from ROS 2;
- log desired and actual joint positions;
- calculate mean and maximum tracking error;
- test at multiple speeds.

### Learn only when needed

- Gazebo model and plugin concepts;
- position, velocity, and effort;
- ros2_control architecture;
- PID intuition;
- sampling rate and tracking error.

### Gate

- the model remains stable when spawned;
- commands do not violate limits;
- tracking results are saved and plotted;
- instability or tuning choices are documented.

## M5: Sensor integration

### Observable behavior

The simulated head moves an RGB-D camera or LiDAR and records data from multiple viewpoints.

### Build tasks

- mount and configure one sensor;
- verify the optical or sensor frame;
- visualize data in RViz;
- record synchronized joint and sensor data;
- create a simple uneven, occluded test world.

### Learn only when needed

- camera or point-cloud messages;
- field of view, range, and resolution;
- sensor frames and timestamps;
- rosbag recording and playback;
- basic point-cloud or depth processing.

### Gate

- the observed scene changes correctly with head motion;
- recorded data can be replayed;
- sensor settings and frames are documented;
- a fixed-forward configuration is available as the baseline.

## M6: Quantitative experiment

### Observable behavior

The same test runner compares fixed and active sensing under controlled conditions and produces result tables.

### Build tasks

- freeze scene, initial pose, sensor settings, and time budget;
- define coverage and obstacle-discovery metrics;
- run at least three scenes;
- repeat each condition;
- retain failed trials;
- export tidy CSV data and plots;
- analyze limitations rather than only the best result.

### Learn only when needed

- experimental controls and baselines;
- mean, spread, and repeated trials;
- metric definitions;
- reproducibility and failure analysis.

### Gate

- one command runs both conditions;
- results are reproducible from saved configuration;
- every figure can be traced to raw data;
- conclusions do not exceed the evidence.

## M7: Portfolio release

### Observable behavior

Another person can reproduce the system and understand what was built, measured, and not validated.

### Build tasks

- clean installation and run instructions;
- architecture diagram;
- two-to-three-minute demo video;
- four-to-six-page English technical report;
- five-slide interview presentation;
- known limitations and next experiments;
- public-release license decision.

### Gate

- clean-environment reproduction succeeds;
- repository has no secrets, absolute personal paths, or unexplained generated code;
- results, video, and report agree;
- the owner can defend design decisions and modify the system live.

## Deferred extensions

These stay deferred until M6 is complete:

- C++ rewrite of one node;
- RTAB-Map;
- adaptive or coverage-aware scan planning;
- physical two-servo prototype;
- low-light or low-texture scenarios;
- MATLAB kinematics cross-check.

## Immediate next action

Start M0 with one observable behavior: generate a pan angle that sweeps from -60 degrees to +60 degrees and back while respecting a configurable angular speed. Do not begin ROS 2 setup until this numerical behavior is tested and understood.
