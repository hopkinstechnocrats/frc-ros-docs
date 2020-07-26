# Cross-compiling ROS for the RoboRIO
These instructions are based on [meta-ros OpenEmbedded Build Instructions](https://github.com/ros/meta-ros/wiki/OpenEmbedded-Build-Instructions). They are intended to be used on a computer running ubuntu, but should also work on other linux distros with some modification.

1. Install dependencies with apt  
`sudo apt-get install gawk wget git-core diffstat unzip texinfo gcc-multilib build-essential chrpath socat cpio python python3-pip python3-pexpect xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev xterm g++-multilib locales lsb-release python3-distutils time`
`sudo locale-gen en_US.utf8`
2. Clone the `meta-ros` repository  
``
