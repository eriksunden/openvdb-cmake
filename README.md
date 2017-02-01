# OpenVDB "easier" multi-platform building

OpenVDB is added as a submodule (to own fork) and minimal OpenEXR/IllmBase as well as zlib has been included for convenience.

The directory named opengl includes glew/glfw as submodules for building openvdb:s internal opengl viewers.

Main contribution is CMake files for easier build environment setups.

Boost and Intel Threading Building Blocks (TBB) is still an external dependency.

Download binaries if possible and set the following environmental variables:

 - BOOST_ROOT   - Path to root folder, example "C:\Boost\boost_1_63_0"

 - TBB_ROOT       - Path to root folder, example "C:\Intel\TBB"

Has been tested on:

 -  Windows 7/8/10, x64 build, Visual Studio (2012/2013/2015)
 -  CMake 2.8.11/3.1.1/3.7.1


