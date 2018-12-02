# hfp\_tcp

>Stream signals from an Airspy HF+ radio to **[SDR Receiver](https://itunes.apple.com/us/app/sdr-receiver/id1289939888?ls=1&mt=8)**, a software defined radio receiver for iOS, or to any application that supports the rtl_tcp protocol. 


## Download and Build hfp_tcp
The hfp\_tcp application is a streaming server that implements the rtl\_tcp protocol for an Airspy HF+ radio.  It enables any application that supports the rtl_tcp protocol to stream data over a network from an Airspy HF+ radio. These directions explain how to build and run the hfp\_tcp application on a Mac or Raspberry Pi.

The hfp\_tcp application consists of a single source file which requires two header files and a dynamic library that are part of the Airspy HF+ Library package.   Building hfp_tcp will generally consist of two steps:

	1. Download and build the Airspy HF+ library.
	2. Download and build hfp_tcp.

The first step will place the dynamic library and header files in standard system locations.  In the second step, the compile and link process will look in these standard locations to locate the header files and dynamic library that are required to build and run hfp_tcp.

If the Airspy SPY Server application has been successfully installed on the Raspberry Pi, Step 1 below will have already been completed and it does not need to repeated.  In this case, skip Step 1 and just follow the directions under Step 2.


### 1. Download and Build the Airspy HF+ Library

The Airspy HF+ Library is an open source  package that includes a library and user mode driver for the Airspy HF+ radio.  The source code and directions for building the package are on GitHub at: [airspy/airspyhf: Code repository for AirspyHF+](https://github.com/airspy/airspyhf/).

To build the library on Raspberry Pi Raspbian, follow the directions provided by Airspy in the repository on GitHub under “How to build the host software on Linux.”  

To build the library on macOS, the first step of the Airspy directions for Linux:

		sudo apt-get install build-essential cmake libusb-1.0-0-dev pkg-config
		
must be replaced by a series of steps which install utilities and libraries that are required to build libairspyhf.  These utilities and libraries are: brew, libusb, cmake, wget and pkg-config.  The initial step of installing brew will also install some other packages including the Xcode Command Line Tools.  The complete sequence is as follows:

**Install Homebrew**

[HomeBrew](https://brew.sh/)

		/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"


**Install libusb**

[libusb](http://macappstore.org/libusb/)  
[How to set up libusb on Mac OS X? - Stack Overflow](https://stackoverflow.com/questions/3853634/how-to-set-up-libusb-on-mac-os-x)

		brew install libusb
		brew link libusb

The link step will attempt to create symbolic links in /usr/local/lib 
for libusb-1.0.0.dylib and libusb-1.0.dylib and some other files, and 
will fail if any of these exist.  If that happens, move or remove the 
files that are reported as already existing.

**Install CMake**

[CMake](https://cmake.org/download/)  
[homebrew - Installing cmake with home-brew - Stack Overflow](https://stackoverflow.com/questions/32185079/installing-cmake-with-home-brew)

		brew install cmake

**Install wget**

		brew install wget


**Install pkg-config**

		brew install pkg-config

**Download and Build libairspyhf**

[airspy/airspyhf: Code repository for AirspyHF+](https://github.com/airspy/airspyhf/)

		wget https://github.com/airspy/airspyhf/archive/master.zip
		unzip master.zip
		cd airspyhf-master 
		mkdir build  
		cd build  
		cmake ../ -DINSTALL_UDEV_RULES=ON  
		make  
		sudo make install

### 2. Download and Build hfp_tcp

Follow the instructions on the green “Clone or Download” drop-down on the right side of the GitHub hfp\_tcp main page to create a local copy of the repository either by cloning it or by downloading the entire repository as a .zip.  Cloning is recommended because it will create a directory containing the entire repository, and subsequent updates can be obtained by executing `git pull`.   Downloading the .zip will result in a directory that contains the source file and the Makefile but not in the form of a Git repository. In either case, from a terminal window, cd to this directory and execute the `make` command.  The hfp\_tcp application will be built.  The terminal log should display Darwin for a build on Mac OS, and Linux for a build on Raspberry Pi, and the command that is being executed for the compile and link will be shown.   If the `make` is to be executed again and there is already an hfp\_tcp executable in the directory, the command `make clean` should be run to remove it.
## Running hfp\_tcp
To run the hfp\_tcp application, connect an Airspy HF+ radio to the system via USB.  The hfp\_tcp application can then be executed from the directory in which it was built with the command `./hfp_tcp`.  By default, the application will listen for an incoming TCP/IP connection on port 1234.  An alternate port can be specified by with the -p flag.  For example, to start the application listening on port 1024, the command is:  `./hfp_tcp -p 1024`.

	 

