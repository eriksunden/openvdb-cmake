# OpenVDB "easier" multi-platform building

OpenVDB is added as a submodule, thus source from dreamworksanimation/openvdb is unmodified.

Minimal OpenEXR/IllmBase and zlib has been included for convenience.

The directory named opengl includes glew/glfw as submodules for building openvdb:s internal opengl viewers.

OpenVDB:s viewer files have been modified to utilize glew.

Main contribution is CMake files for easier build environment setups.

Boost and Intel Threading Building Blocks (TBB) is still an external dependency.

Download binaries if possible and set the following environmental variables:

 - BOOST_ROOT   - Path to root folder, example "C:\Boost\boost_1_57_0"

 - TBB_ROOT       - Path to root folder, example "C:\Intel\tbb43_20141204oss"

Has been tested on:

 -  Windows 7/8, x64 build, Visual Studio (2012/2013)
 -  CMake 2.8.11 and 3.1.1


