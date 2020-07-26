# Cross-compiling ROS for the RoboRIO
These instructions are based on [meta-ros OpenEmbedded Build Instructions](https://github.com/ros/meta-ros/wiki/OpenEmbedded-Build-Instructions). They are intended to be used on a computer running ubuntu, but should also work on other linux distros with some modification.

1. Install dependencies with apt  
```
sudo apt-get install gawk wget git-core diffstat unzip texinfo gcc-multilib build-essential chrpath socat cpio python python3-pip python3-pexpect xz-utils debianutils iputils-ping python3-git python3-jinja2 libegl1-mesa libsdl1.2-dev xterm g++-multilib locales lsb-release python3-distutils time
sudo locale-gen en_US.utf8
```
2. Create a directory for this project and enter the directory  
```
mkdir ~/ros-roborio && cd ~/ros-roborio	
```
3. Clone the `meta-ros` repository and setup the `conf` directory  
	git clone https://github.com/ros/meta-ros.git
	mkdir conf
	ln -snf ../conf build/.
4. Checkout the `build-draft` branch of the `meta-ros` repository  
	cd meta-ros && git checkout build-draft && cd -
5. Set environment variables  
	export distro=ros1
	export ros_distro=melodic
	export oe_release_series=dunfell
6. Generate configuration
	cfg=$distro-$ros_distro-$oe_release_series.mcf
	cp meta-ros/files/$cfg conf/.
	meta-ros/scripts/mcf -f conf/$cfg
7. Set up the shell environment for this build and create a conf/local.conf
	unset BDIR BITBAKEDIR BUILDDIR OECORELAYERCONF OECORELOCALCONF OECORENOTESCONF OEROOT TEMPLATECONF
	source openembedded-core/oe-init-build-env && cd -
8. Add this to the bottom of `~/ros-roborio/conf/local.conf`
	# ROS-ADDITIONS-BEGIN
	# ^^^^^^^^^^^^^^^^^^^ In the future, tools will expect to find this line.
	
	# Increment the minor version whenever you add or change a setting in this file.
	ROS_LOCAL_CONF_SETTINGS_VERSION = "2.1"
	
	# If not using mcf, replace ${MCF_DISTRO} with the DISTRO being used.
	DISTRO = "${MCF_DISTRO}"

	# If not using mcf, set ROS_DISTRO in conf/bblayers.conf .
	
	# The list of supported values of MACHINE is found in the Machines[] array in the .mcf file for the selected configuration.
	# Use ?= so that a value set in the environment will override the one set here.
	MACHINE ?= "cortexa9-zynq"

	# Can remove if DISTRO is "webos". If not using mcf, replace ${MCF_OPENEMBEDDED_VERSION} with the version of OpenEmbedded
	# being used. See the comments in files/ros*.mcf for its format.
	ROS_DISTRO_VERSION_APPEND = "+${MCF_OPENEMBEDDED_VERSION}"
	
	# Can remove if DISTRO is not "webos". If not using mcf, replace ${MCF_WEBOS_BUILD_NUMBER} with the build number of webOS OSE
	# being used.
	ROS_WEBOS_DISTRO_VERSION_APPEND = ".${MCF_WEBOS_BUILD_NUMBER}"
	
	# If not using mcf, replace ${MCF_OE_RELEASE_SERIES} with the OpenEmbedded release series being used.
	ROS_OE_RELEASE_SERIES_SUFFIX = "-${MCF_OE_RELEASE_SERIES}"
	
	# Because of a bug in OpenEmbedded, <ABSOLUTE-PATH-TO-DIRECTORY-ON-SEPARATE-DISK> can not be a symlink.
	ROS_COMMON_ARTIFACTS = "<ABSOLUTE-PATH-TO-DIRECTORY-ON-SEPARATE-DISK>"

	# Set the directories where downloads, shared-state, and the output from the build are placed to be on the separate disk.
	DL_DIR = "${ROS_COMMON_ARTIFACTS}/downloads"
	SSTATE_DIR = "${ROS_COMMON_ARTIFACTS}/sstate-cache${ROS_OE_RELEASE_SERIES_SUFFIX}"
	TMPDIR = "${ROS_COMMON_ARTIFACTS}/BUILD-${DISTRO}-${ROS_DISTRO}${ROS_OE_RELEASE_SERIES_SUFFIX}"
	# Don't add the libc variant suffix to TMPDIR.
	TCLIBCAPPEND := ""

	# As recommended by https://www.yoctoproject.org/docs/latest/mega-manual/mega-manual.html#var-BB_NUMBER_THREADS
	# and https://www.yoctoproject.org/docs/latest/mega-manual/mega-manual.html#var-PARALLEL_MAKE:
	BB_NUMBER_THREADS = "${@min(int(bb.utils.cpu_count()), 20)}"
	PARALLEL_MAKE = "-j ${BB_NUMBER_THREADS}"

	# Reduce the size of the build artifacts by removing the working files under TMPDIR/work. Comment this out to preserve them
	# (see https://www.yoctoproject.org/docs/latest/mega-manual/mega-manual.html#ref-classes-rm-work).
	INHERIT += "rm_work"
	
	
	# Any other additions to the file go here.
	
	# vvvvvvvvvvvvvvvvv In the future, tools will expect to find this line.
	# ROS-ADDITIONS-END
