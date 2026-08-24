# Autonomous-Docking-with-TurtleBot3-Using-Real-Time-LED-Tracking
This project implements a ROS 1 pipeline to detect, track, and drive toward LED markers using a calibrated camera, real-time visual feedback, and closed-loop distance control.

## Overview

- **Platform:** TurtleBot3 running on ROS
- **Objective:** Detect blinking LED markers, estimate real-time distance using camera calibration, and autonomously drive the robot to stop at a predefined threshold of 24 inches
- **Core Technologies:** ROS 1, Python, OpenCV, Normalized Correlation, Image Differencing, Closed-Loop Motor Control

## Key Features

### Visual Perception & LED Tracking
- Subscribes to the robot's camera topic within a synchronized multi-node ROS network
- Uses blinking LED detection (image differencing) to identify initial X, Y coordinates of three LEDs during a stationary phase
- Transitions to normalized correlation tracking for robust real-time tracking at 30 fps while the robot is in motion
- Maintains strong template-matching confidence (correlation coefficient > 0.90) throughout the run

### Distance Estimation & Control
- Converts pixel distance between front LEDs into physical distance (inches) using pre-derived linear regression coefficients (slope and offset)
- Continuously evaluates estimated distance against a 24-inch stop threshold
- Commands forward velocity (0.1 m/s) while above threshold; issues a stop command once within range
- Uses rear-LED X-offset relative to the front LED midpoint to compute a turn/steering correction, enabling the robot to re-center itself while approaching


## Results

- TurtleBot3 successfully tracked LED markers and autonomously approached the target from an initial distance of 72 inches
- Achieved a stop distance of approximately 22 inches, close to the 24-inch target threshold
- Successfully demonstrated steering correction across three starting positions (center-aimed, center-aimed left, offset-left)
- Wheel speeds constrained to 0.1 m/s to stay within hardware limits (0.22 m/s), preventing motor stalling during turns

## Dependencies

- ROS Noetic
- Python 3
- `rospy`, `cv_bridge`, `numpy`, `opencv-python`
- `rosserial`, `sensor_msgs`, `geometry_msgs`

## How to Run

1. Launch the robot and camera:
   ```bash
    roslaunch turtlebot3_bringup turtlebot3_robot.launch


2. Activate the LED Raspberry Pi node for initial blinking:
   ```bash
     rosrun your_package_name led_activate.py


3. Run the integrated tracking, distance estimation, and control script:
    ```bash
    rosrun your_package_name sync_drive_LED.py
    
