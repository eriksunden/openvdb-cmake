# The MIT License (MIT)

# Copyright (c) 2015 Erik Sundén

# Permission is hereby granted, free of charge, to any person obtaining a copy
# of this software and associated documentation files (the "Software"), to deal
# in the Software without restriction, including without limitation the rights
# to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
# copies of the Software, and to permit persons to whom the Software is
# furnished to do so, subject to the following conditions:

# The above copyright notice and this permission notice shall be included in all
# copies or substantial portions of the Software.

# THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
# IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
# FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
# AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
# LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
# OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
# SOFTWARE.

cmake_minimum_required(VERSION 2.8.11)
cmake_policy(VERSION 2.8.11)

if(POLICY CMP0046)
    cmake_policy(SET CMP0046 OLD) # Missing dependencies policy.
endif()

if(POLICY CMP0043)
    cmake_policy(SET CMP0043 OLD) # If compile defs should be ignored.
endif()

if(CMAKE_VERSION VERSION_GREATER "3.0.0")
	cmake_policy(SET CMP0042 OLD) # MacOSX rpath policy.
endif()

if (NOT CMAKE_VERSION VERSION_LESS "3.1.0")
	# include TARGET_OBJECTS expressions in a target's SOURCES property
	cmake_policy(SET CMP0051 NEW)
	# only interpret if() arguments as variables or keywords when unquoted
	cmake_policy(SET CMP0054 NEW)
endif()

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}/cmake/")

# OpenVDB Package
project(OpenVDB)

#--------------------------------------------------------------------
# Suppress warnings etc
if(WIN32)
add_definitions( "/W0 /D_CRT_SECURE_NO_WARNINGS /wd4005 /wd4221 /wd4996 /nologo /IGNORE:4006" )
    if (MSVC)
        set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} /MT")
        set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} /MTd")
        if (CMAKE_CL_64)
            set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /bigobj")
        endif ()
        add_definitions(-DZLIB_WINAPI)
        add_definitions(-DNOMINMAX)
    endif ()
endif()

#--------------------------------------------------------------------
# Include OpenVDB directory
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/openvdb_dw/)

#--------------------------------------------------------------------
# Add OPENVDB header files
file(GLOB_RECURSE IO_HEADER_FILES ${CMAKE_CURRENT_SOURCE_DIR}/openvdb_dw/openvdb/io/*.h)
file(GLOB_RECURSE MATH_HEADER_FILES ${CMAKE_CURRENT_SOURCE_DIR}/openvdb_dw/openvdb/math/*.h)
file(GLOB_RECURSE METADATA_HEADER_FILES ${CMAKE_CURRENT_SOURCE_DIR}/openvdb_dw/openvdb/metadata/*.h)
file(GLOB_RECURSE TOOLS_HEADER_FILES ${CMAKE_CURRENT_SOURCE_DIR}/openvdb_dw/openvdb/tools/*.h)
file(GLOB_RECURSE TREE_HEADER_FILES ${CMAKE_CURRENT_SOURCE_DIR}/openvdb_dw/openvdb/tree/*.h)
file(GLOB_RECURSE UTILS_HEADER_FILES ${CMAKE_CURRENT_SOURCE_DIR}/openvdb_dw/openvdb/util/*.h)

set(OPENVDB_HEADER_FILES 
				openvdb_dw/openvdb/Exceptions.h
				openvdb_dw/openvdb/Grid.h
				openvdb_dw/openvdb/Metadata.h
				openvdb_dw/openvdb/openvdb.h
				openvdb_dw/openvdb/Platform.h
				openvdb_dw/openvdb/PlatformConfig.h
				openvdb_dw/openvdb/Types.h
                openvdb_dw/openvdb/version.h)

LIST(APPEND OPENVDB_HEADER_FILES 	${IO_HEADER_FILES} 
							${MATH_HEADER_FILES} 
							${METADATA_HEADER_FILES} 
							${TOOLS_HEADER_FILES}
                            ${TREE_HEADER_FILES}
							${UTILS_HEADER_FILES})
                            
#--------------------------------------------------------------------
# Add OPENVDB source files
file(GLOB_RECURSE IO_SOURCE_FILES ${CMAKE_CURRENT_SOURCE_DIR}/openvdb_dw/openvdb/io/*.cc)
file(GLOB_RECURSE MATH_SOURCE_FILES ${CMAKE_CURRENT_SOURCE_DIR}/openvdb_dw/openvdb/math/*.cc)
file(GLOB_RECURSE METADATA_SOURCE_FILES ${CMAKE_CURRENT_SOURCE_DIR}/openvdb_dw/openvdb/metadata/*.cc)
file(GLOB_RECURSE UTILS_SOURCE_FILES ${CMAKE_CURRENT_SOURCE_DIR}/openvdb_dw/openvdb/util/*.cc)

set(OPENVDB_SOURCE_FILES 
				openvdb_dw/openvdb/Grid.cc
				openvdb_dw/openvdb/openvdb.cc
				openvdb_dw/openvdb/Platform.cc)

LIST(APPEND OPENVDB_SOURCE_FILES 	${IO_SOURCE_FILES} 
							${MATH_SOURCE_FILES} 
							${METADATA_SOURCE_FILES}
							${UTILS_SOURCE_FILES})
                            
#--------------------------------------------------------------------
# Include ext directory
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/ext/)

#--------------------------------------------------------------------
# Add OpenEXR files
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/ext/OpenEXR/)

set(HALF_HEADER_FILES
    ext/OpenEXR/eLut.h
    ext/OpenEXR/half.h
    ext/OpenEXR/halfFunction.h
    ext/OpenEXR/halfLimits.h
    ext/OpenEXR/toFloat.h
)
set(HALF_SOURCE_FILES
    ext/OpenEXR/eLut.cpp
    ext/OpenEXR/half.cpp
    ext/OpenEXR/toFloat.cpp
)

file(GLOB_RECURSE IEX_HEADER_FILES ${CMAKE_CURRENT_SOURCE_DIR}/ext/OpenEXR/Ie*.h)
file(GLOB_RECURSE IEX_SOURCE_FILES ${CMAKE_CURRENT_SOURCE_DIR}/ext/OpenEXR/Ie*.cpp)

file(GLOB_RECURSE ILLMIMF_HEADER_FILES ${CMAKE_CURRENT_SOURCE_DIR}/ext/OpenEXR/Imf*.h)
file(GLOB_RECURSE ILLMIMF_SOURCE_FILES ${CMAKE_CURRENT_SOURCE_DIR}/ext/OpenEXR/Imf*.cpp)
list(APPEND ILLMIMF_HEADER_FILES ${CMAKE_CURRENT_SOURCE_DIR}/ext/OpenEXR/b44ExpLogTable.h)

set(ILMTHREAD_HEADER_FILES
    ext/OpenEXR/IlmThread.h
    ext/OpenEXR/IlmThreadMutex.h
    ext/OpenEXR/IlmThreadPool.h
    ext/OpenEXR/IlmThreadSemaphore.h
)
if(WIN32)
set(ILMTHREAD_SOURCE_FILES
    ext/OpenEXR/IlmThreadPool.cpp
    ext/OpenEXR/IlmThreadPosix.cpp
    ext/OpenEXR/IlmThreadPool.cpp
    ext/OpenEXR/IlmThreadMutexWin32.cpp
    ext/OpenEXR/IlmThreadSemaphoreWin32.cpp
    ext/OpenEXR/IlmThreadWin32.cpp
)
else()
set(ILMTHREAD_SOURCE_FILES
    ext/OpenEXR/IlmThread.cpp
    ext/OpenEXR/IlmThreadMutex.cpp
    ext/OpenEXR/IlmThreadMutexPosix.cpp
    ext/OpenEXR/IlmThreadPool.cpp
    ext/OpenEXR/IlmThreadPosix.cpp
    ext/OpenEXR/IlmThreadPool.cpp
    ext/OpenEXR/IlmThreadSemaphore.cpp
    ext/OpenEXR/IlmThreadSemaphorePosix.cpp
    ext/OpenEXR/IlmThreadSemaphorePosixCompat.cpp
)
endif()

file(GLOB_RECURSE IMATH_HEADER_FILES ${CMAKE_CURRENT_SOURCE_DIR}/ext/OpenEXR/Imath*.h)
file(GLOB_RECURSE IMATH_SOURCE_FILES ${CMAKE_CURRENT_SOURCE_DIR}/ext/OpenEXR/Imath*.cpp)

set(OPENEXR_HEADER_FILES ${HALF_HEADER_FILES} ${IEX_HEADER_FILES} ${ILLMIMF_HEADER_FILES} ${ILMTHREAD_HEADER_FILES} ${CMAKE_CURRENT_SOURCE_DIR}/ext/OpenEXR/IlmBaseConfig.h
    ${CMAKE_CURRENT_SOURCE_DIR}/ext/OpenEXR/OpenEXRConfig.h)
set(OPENEXR_SOURCE_FILES ${HALF_SOURCE_FILES} ${IEX_SOURCE_FILES} ${ILLMIMF_SOURCE_FILES} ${ILMTHREAD_SOURCE_FILES} ${CMAKE_CURRENT_SOURCE_DIR}/ext/OpenEXR/asd.cpp)

#--------------------------------------------------------------------
# Add ZLib files
file(GLOB_RECURSE ZLIB_HEADER_FILES ${CMAKE_CURRENT_SOURCE_DIR}/ext/zlib/*.h)
file(GLOB_RECURSE ZLIB_SOURCE_FILES ${CMAKE_CURRENT_SOURCE_DIR}/ext/zlib/*.c)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/ext/zlib/)

#--------------------------------------------------------------------
# Find Boost
set(BOOST_ROOT $ENV{BOOST_ROOT} CACHE STRING "Boost root")
if(NOT BOOST_LIBRARYDIR)
  set(BOOST_LIBRARYDIR "")
  file(GLOB children RELATIVE ${BOOST_ROOT} ${BOOST_ROOT}/*)
  foreach(child ${children})
    if((IS_DIRECTORY ${BOOST_ROOT}/${child}) AND (${child} MATCHES "lib(.*)"))
        file(GLOB lib_children RELATIVE ${BOOST_ROOT}/${child} ${BOOST_ROOT}/${child}/*)
        foreach(lib_child ${lib_children})
            if((NOT IS_DIRECTORY ${BOOST_ROOT}/${child}/${lib_child}) AND (${lib_child} MATCHES "libboost(.*)"))
                message("Boost libs found in ${BOOST_ROOT}\\${child}")
                set(BOOST_LIBRARYDIR ${BOOST_ROOT}\\${child} CACHE STRING "Boost libs")
                set(FOUND_LIB TRUE)
            endif()
            if(FOUND_LIB)
                break()
            endif()
        endforeach()
    endif()
    if(FOUND_LIB)
        break()
    endif()
  endforeach()
endif()

find_package(Boost COMPONENTS system thread)

if(NOT Boost_FOUND)
        MESSAGE (STATUS "Boost not found.")
endif()

if(NOT Boost_LIBRARY_DIRS)
        set(Boost_LIBRARY_DIRS ${BOOST_LIBRARYDIR})
endif()

mark_as_advanced(CLEAR Boost_INCLUDE_DIRS)
mark_as_advanced(CLEAR Boost_LIBRARY_DIRS)

include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})

#--------------------------------------------------------------------
# Find Intel Threading Building Blocks
set(FIND_LIBRARY_USE_LIB64_PATHS ON)
find_package(TBB)
if(NOT TBB_FOUND)
        MESSAGE (STATUS "TBB not found.")
endif()
set(TBB_LIBRARIES optimized ${TBB_LIBRARY} debug ${TBB_LIBRARY_DEBUG})

mark_as_advanced(CLEAR TBB_INCLUDE_DIRS)
mark_as_advanced(CLEAR TBB_LIBRARY_DIRS)

include_directories(${TBB_INCLUDE_DIRS})
link_directories(${TBB_LIBRARY_DIRS})

#--------------------------------------------------------------------
# Define defintions
remove_definitions(-DHAVE_CONFIG_H)
add_definitions(-DOPENVDB_TOOLS_RAYTRACER_USE_EXR)
if(NOT BUILD_SHARED_LIBS)
    add_definitions(-DOPENVDB_LIB)
else()
    add_definitions(-DHALF_EXPORTS)
endif()

#--------------------------------------------------------------------
# Create library
add_library(openvdb STATIC ${OPENVDB_SOURCE_FILES} ${OPENEXR_SOURCE_FILES} ${ZLIB_SOURCE_FILES} ${OPENVDB_HEADER_FILES} ${OPENEXR_HEADER_FILES} ${ZLIB_HEADER_FILES})

#--------------------------------------------------------------------
# Dependency libs
target_link_libraries(openvdb ${TBB_LIBRARIES})