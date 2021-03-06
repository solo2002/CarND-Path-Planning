# CarND-Path-Planning-Project
Self-Driving Car Engineer Nanodegree Program
   
### Simulator.
You can download the Term3 Simulator which contains the Path Planning Project from the [releases tab (https://github.com/udacity/self-driving-car-sim/releases/tag/T3_v1.2).

### Goals
In this project your goal is to safely navigate around a virtual highway with other traffic that is driving +-10 MPH of the 50 MPH speed limit. You will be provided the car's localization and sensor fusion data, there is also a sparse map list of waypoints around the highway. The car should try to go as close as possible to the 50 MPH speed limit, which means passing slower traffic when possible, note that other cars will try to change lanes too. The car should avoid hitting other cars at all cost as well as driving inside of the marked road lanes at all times, unless going from one lane to another. The car should be able to make one complete loop around the 6946m highway. Since the car is trying to go 50 MPH, it should take a little over 5 minutes to complete 1 loop. Also the car should not experience total acceleration over 10 m/s^2 and jerk that is greater than 10 m/s^3.

#### The map of the highway is in data/highway_map.txt
Each waypoint in the list contains  [x,y,s,dx,dy] values. x and y are the waypoint's map coordinate position, the s value is the distance along the road to get to that waypoint in meters, the dx and dy values define the unit normal vector pointing outward of the highway loop.

The highway's waypoints loop around so the frenet s value, distance along the road, goes from 0 to 6945.554.

## Basic Build Instructions

1. Clone this repo.
2. Make a build directory: `mkdir build && cd build`
3. Compile: `cmake .. && make`
4. Run it: `./path_planning`.

Here is the data provided from the Simulator to the C++ Program

#### Main car's localization Data (No Noise)

["x"] The car's x position in map coordinates

["y"] The car's y position in map coordinates

["s"] The car's s position in frenet coordinates

["d"] The car's d position in frenet coordinates

["yaw"] The car's yaw angle in the map

["speed"] The car's speed in MPH

#### Previous path data given to the Planner

//Note: Return the previous list but with processed points removed, can be a nice tool to show how far along
the path has processed since last time. 

["previous_path_x"] The previous list of x points previously given to the simulator

["previous_path_y"] The previous list of y points previously given to the simulator

#### Previous path's end s and d values 

["end_path_s"] The previous list's last point's frenet s value

["end_path_d"] The previous list's last point's frenet d value

#### Sensor Fusion Data, a list of all other car's attributes on the same side of the road. (No Noise)

["sensor_fusion"] A 2d vector of cars and then that car's [car's unique ID, car's x position in map coordinates, car's y position in map coordinates, car's x velocity in m/s, car's y velocity in m/s, car's s position in frenet coordinates, car's d position in frenet coordinates. 

---

## Dependencies

* cmake >= 3.5
  * All OSes: [click here for installation instructions](https://cmake.org/install/)
* make >= 4.1
  * Linux: make is installed by default on most Linux distros
  * Mac: [install Xcode command line tools to get make](https://developer.apple.com/xcode/features/)
  * Windows: [Click here for installation instructions](http://gnuwin32.sourceforge.net/packages/make.htm)
* gcc/g++ >= 5.4
  * Linux: gcc / g++ is installed by default on most Linux distros
  * Mac: same deal as make - [install Xcode command line tools]((https://developer.apple.com/xcode/features/)
  * Windows: recommend using [MinGW](http://www.mingw.org/)
* [uWebSockets](https://github.com/uWebSockets/uWebSockets)
  * Run either `install-mac.sh` or `install-ubuntu.sh`.
  * If you install from source, checkout to commit `e94b6e1`, i.e.
    ```
    git clone https://github.com/uWebSockets/uWebSockets 
    cd uWebSockets
    git checkout e94b6e1
    ```

## Implementation

### Compilation

The code complies without errors with ```cmake```. The ```CMakeLists.txt``` has not been changed. There is only one new head file added, spline.h, from [here](http://kluge.in-chemnitz.de/opensource/spline/), which is required for generating trajectory.  

### Valid Trajectories

The car is able to drive more than 5 miles without any incidents.
![alt test](https://github.com/solo2002/CarND-Path-Planning/blob/master/img/mileage.png)

The full video is available at [here](https://youtu.be/K5eAXkJaPaM). In addition, the car doesn't drive faster than the speed limit. Also the car isn't driving much slower than speed limit unless obstructed by traffic. THe car's accelerations and jerks are not exceeded, and stays in its lane most of time except changing lanes.

### Relection

The implementation of path planning algorithm is in the main.cpp under the src folder between line 262 to 442.

#### 1. Detecting Cars

Here sensoe fusion data and telemetry data are used to deteremine whether there are cars in the near front (no more than 30 meters) of the target car, cars in the near right, and cars in the near left (line 262 to 290). The program examines whether the car should accelerate, slow-down, keep the lane, change lane to left, or change lane to right based on 3 variables (from lane 291 to 345). 

#### 2. Generating Path

I am using the strategy (with a few improments) that is introduced in the project walkthrough and Q/A video. Basically, spline is used to generate the trajectory with the car coordinates and previous path points (from line 346 to 442, where a transformation of the map coordinates and car's own coordinates is implemented). First of all, the trajectory (which is showing as a green line in the video) is initialized with the target car position, and then the last 2 points of previous trajectory are used and combining with 3 points at farther distance. The total waypoints are set to 50. Here, the helper function, getXY() is used to generate 3 points which are evenly separated at 30 meters in front of car. Spline function is really helpful to generate a smooth trajectory. Moreover, the unused waypoints of the previous trajectory would be save to the current one to make the trajectory transforming smooothly. 






































