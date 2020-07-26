# Cross-compiling ROS for the RoboRIO
These instructions are based on [meta-ros OpenEmbedded Build Instructions](https://github.com/ros/meta-ros/wiki/OpenEmbedded-Build-Instructions). They are intended to be used on a computer running ubuntu, but should also work on other linux distros with some modification.

1. Install dependencies with apt  
`sudo apt-get install gawk wget git-core diffstat unzip texinfo gcc-multilib build-essential chrpath socat cpio python python3-pip python3-pexpect xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev xterm g++-multilib locales lsb-release python3-distutils time`  
`sudo locale-gen en_US.utf8`
2. Create a directory for this project and enter the directory  
`mkdir ~/ros-roborio && cd ~/ros-roborio`
3. Clone the `meta-ros` repository  
`git clone https://github.com/ros/meta-ros.git`
4. Checkout the `build-draft` branch of the `meta-ros` repository
`cd meta-ros && git checkout build-draft && cd -`
5. Set environment variables
 * `export distro=<DISTRO>`
  * replace <DISTRO> with `ros1`,`ros2`, or `webos`
 * `export ros_distro=<ROS_DISTRO>`
  * replace <ROS_DISTRO> with `melodic`, `dashing`, or `eloquent`
 * `export oe_release_series=dunfell`
6. Generate configuration
`cfg=$distro-$ros_distro-$oe_release_series.mcf`  
`cp meta-ros/files/$cfg conf/.`  
`meta-ros/scripts/mcf -f conf/$cfg`
7. Set up the shell environment for this build and create a conf-local.conf
`unset BDIR BITBAKEDIR BUILDDIR OECORELAYERCONF OECORELOCALCONF OECORENOTESCONF OEROOT TEMPLATECONF`
`source openembedded-core/oe-init-build-env`
