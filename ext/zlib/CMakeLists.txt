project(zlib)

#--------------------------------------------------------------------
# Add ZLib files
file(GLOB_RECURSE ZLIB_HEADER_FILES ${CMAKE_CURRENT_SOURCE_DIR}/*.h)
file(GLOB_RECURSE ZLIB_SOURCE_FILES ${CMAKE_CURRENT_SOURCE_DIR}/*.c)
include_directories(${CMAKE_CURRENT_SOURCE_DIR})

#--------------------------------------------------------------------
# Define defintions
if(BUILD_SHARED_LIBS)
    add_definitions(-DZLIB_DLL)
    add_definitions(-DZLIB_INTERNAL)
endif()

#--------------------------------------------------------------------
# Create library
add_library(zlib ${ZLIB_HEADER_FILES} ${ZLIB_SOURCE_FILES})