# OpenVDB cross-platform build (made easier)

OpenVDB is included as a submodule, thus no source from dreamworksanimation/openvdb has been modified.
Main contribution is CMake files for easier build environment setups.
Minimal OpenEXR/IllmBase and zlib has been included for convenience.

Boost and Intel Threading Building Blocks (TBB) is still an external dependency.
Download and set the following environmental variables:

BOOST_ROOT   - Path to root folder, example "C:\Boost\boost_1_57_0"
TBB_ROOT       - Path to root folder, example "C:\Intel\tbb43_20141204oss"

Has been tested on Windows 7/8, x64 build, Visual Studio (2012/2013)


