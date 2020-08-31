# Repository and Package Structure 

## Github Repositories

#### ROS Package Repositories

* Each ROS package created will have its own Github repository.

#### `meta-technocrats` Bitbake Layer Repository

* This repository will be a Bitbake layer containing recipes for each ROS Package
* It will also specify the MACHINE variable and the correct TUNE_FEATURES for the RoboRIO's processor

#### Robot Code Launcher Repository

* Top-level configuration repository for robot code.
* Contains:
  * roslaunch files to start robot code
  * scripts for deploying code, installing ipks, etc.

## Development Work flow

1. Make changes to a package.
2. Push changes to Github.
3. Run bitbake to build packages.
   1. *TODO: Check whether bitbake can see if Github repositories have been updated and only rebuild packages with changes.*
4. Copy package ipks to RoboRIO with script for the Launcher repository.
5. Install ipks on the RoboRIO using opkg.
6. Copy roslaunch files from the Launcher repository onto the RoboRIO.
7. Use ssh to run roslaunch

