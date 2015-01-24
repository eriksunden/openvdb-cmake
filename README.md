# OpenVDB "easier" multi-platform building

OpenVDB is added as a submodule, thus source from dreamworksanimation/openvdb is unmodified.

Minimal OpenEXR/IllmBase and zlib has been included for convenience.

Master branch contains only the necessary setup for building the openvdb library. 

Upcoming branch 'opengl' will include glew/glfw for building openvdb:s internal opengl viewers.

Main contribution is CMake files for easier build environment setups.

Download CMake >=2.8.11 (not tested with CMake 3.* yet)

Boost and Intel Threading Building Blocks (TBB) is still an external dependency.

Download and set the following environmental variables:

BOOST_ROOT   - Path to root folder, example "C:\Boost\boost_1_57_0"

TBB_ROOT       - Path to root folder, example "C:\Intel\tbb43_20141204oss"

Has been tested on Windows 7/8, x64 build, Visual Studio (2012/2013)


