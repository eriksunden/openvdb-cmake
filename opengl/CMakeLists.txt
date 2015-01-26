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

option(BUILD_GLVIEWER "Build OpenVDB OpenGL Viewer" OFF)
option(BUILD_GLRENDERER "Build OpenVDB OpenGL Renderer" OFF)

if(BUILD_GLVIEWER OR BUILD_GLRENDERER)

if(BUILD_SHARED_LIBS AND BUILD_GLRENDERER)
    set(BUILD_GLRENDERER OFF CACHE BOOL "Build OpenVDB OpenGL Renderer" FORCE)
    message("OpenVDB RayTracer require static libs as all functions are not exported")
endif()

#--------------------------------------------------------------------
# Build deps (GLFW and GLEW)
add_subdirectory(ext)

#--------------------------------------------------------------------
# Find OpenGL
find_package(OpenGL REQUIRED)
    
#--------------------------------------------------------------------
# Add Viewer files
file(GLOB_RECURSE VIEWER_FILES ${CMAKE_CURRENT_SOURCE_DIR}/openvdb_viewer/*)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../)
include_directories(${CMAKE_CURRENT_SOURCE_DIR})
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/ext/glfw/include)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/ext/glew/include)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../ext/OpenEXR)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../ext/zlib)

#--------------------------------------------------------------------
# Add defs and create library
add_definitions(-DOPENVDB_USE_GLFW_3)
add_definitions(-DOPENVDB_USE_GLEW)

if(NOT BUILD_SHARED_LIBS)
add_definitions(-DGLEW_STATIC)
endif()
add_definitions( "/W0 /D_CRT_SECURE_NO_WARNINGS /wd4005 /wd4221 /wd4996 /nologo" )

#--------------------------------------------------------------------
# Link and include externals
include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})
include_directories(${TBB_INCLUDE_DIRS})
link_directories(${TBB_LIBRARY_DIRS})
include_directories(${OPENGL_INCLUDE_DIR})

#--------------------------------------------------------------------
# Dependency libs
if(WIN32)
    link_directories(${CMAKE_BINARY_DIR}/opengl/ext/glew/Debug/)
    link_directories(${CMAKE_BINARY_DIR}/opengl/ext/glew/Release/)
    link_directories(${CMAKE_BINARY_DIR}/opengl/ext/glfw/src/Debug/)
    link_directories(${CMAKE_BINARY_DIR}/opengl/ext/glfw/src/Release/)
if(BUILD_SHARED_LIBS)
    set(GLFW_LIBRARIES glfw3dll)
else()
    set(GLFW_LIBRARIES glfw3)
endif()
endif()

set(GLEW_LIBRARIES glew)

if(BUILD_GLVIEWER)
    #--------------------------------------------------------------------
    # Create library
    add_executable(openvdb_app_glviewer MACOSX_BUNDLE WIN32 ${CMAKE_CURRENT_SOURCE_DIR}/../openvdb_dw/openvdb/cmd/openvdb_view/main.cc ${VIEWER_FILES})
    
    if(WIN32 AND MSVC)
       set_target_properties(openvdb_app_glviewer PROPERTIES LINK_FLAGS_DEBUG "/SUBSYSTEM:CONSOLE")
       set_target_properties(openvdb_app_glviewer PROPERTIES COMPILE_DEFINITIONS_DEBUG "_CONSOLE")
       set_target_properties(openvdb_app_glviewer PROPERTIES LINK_FLAGS_RELWITHDEBINFO "/SUBSYSTEM:CONSOLE")
       set_target_properties(openvdb_app_glviewer PROPERTIES COMPILE_DEFINITIONS_RELWITHDEBINFO "_CONSOLE")
       set_target_properties(openvdb_app_glviewer PROPERTIES LINK_FLAGS_RELEASE "/SUBSYSTEM:CONSOLE")
       set_target_properties(openvdb_app_glviewer PROPERTIES COMPILE_DEFINITIONS_RELEASE "_CONSOLE")
       if(NOT BUILD_SHARED_LIBS)
            set_target_properties(openvdb_app_glviewer PROPERTIES LINK_FLAGS "/FORCE:MULTIPLE  /IGNORE:4006")
       endif()
    endif()

    #--------------------------------------------------------------------
    # Dependency libs
    add_dependencies(openvdb_app_glviewer glew glfw openvdb)
    target_link_libraries(openvdb_app_glviewer ${GLEW_LIBRARIES} ${OPENGL_LIBRARIES} ${GLFW_LIBRARIES} openvdb openexr zlib ${Boost_LIBRARIES} ${TBB_LIBRARIES})
endif()

if(BUILD_GLRENDERER)
    #--------------------------------------------------------------------
    # Create library
    add_executable(openvdb_app_glrenderer MACOSX_BUNDLE WIN32 ${CMAKE_CURRENT_SOURCE_DIR}/../openvdb_dw/openvdb/cmd/openvdb_render/main.cc ${VIEWER_FILES})
    
    if(WIN32 AND MSVC)
       set_target_properties(openvdb_app_glrenderer PROPERTIES LINK_FLAGS_DEBUG "/SUBSYSTEM:CONSOLE")
       set_target_properties(openvdb_app_glrenderer PROPERTIES COMPILE_DEFINITIONS_DEBUG "_CONSOLE")
       set_target_properties(openvdb_app_glrenderer PROPERTIES LINK_FLAGS_RELWITHDEBINFO "/SUBSYSTEM:CONSOLE")
       set_target_properties(openvdb_app_glrenderer PROPERTIES COMPILE_DEFINITIONS_RELWITHDEBINFO "_CONSOLE")
       set_target_properties(openvdb_app_glrenderer PROPERTIES LINK_FLAGS_RELEASE "/SUBSYSTEM:CONSOLE")
       set_target_properties(openvdb_app_glrenderer PROPERTIES COMPILE_DEFINITIONS_RELEASE "_CONSOLE")
       if(NOT BUILD_SHARED_LIBS)
            set_target_properties(openvdb_app_glrenderer PROPERTIES LINK_FLAGS "/FORCE:MULTIPLE  /IGNORE:4006")
       endif()
    endif()

    #--------------------------------------------------------------------
    # Dependency libs
    add_dependencies(openvdb_app_glrenderer glew glfw openvdb)
    target_link_libraries(openvdb_app_glrenderer ${GLEW_LIBRARIES} ${OPENGL_LIBRARIES} ${GLFW_LIBRARIES} openvdb openexr zlib ${Boost_LIBRARIES} ${TBB_LIBRARIES})
endif()
endif()