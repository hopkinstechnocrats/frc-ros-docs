# Repository and Package Structure 

## Github Repositories

#### ROS Package Repositories

* Each ROS package created will have its own Github repository.

#### `meta-technocrats` Bitbake Layer Repository

* This repository will be a Bitbake layer containing recipes for each ROS Package

#### Robot Code Launcher Repository

* Top-level configuration repository for robot code.
* Contains:
  * roslaunch files to start robot code
  * scripts for deploying code, installing ipks, etc.