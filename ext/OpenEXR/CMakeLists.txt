project(openexr)

#--------------------------------------------------------------------
# Include directories
include_directories(${CMAKE_CURRENT_SOURCE_DIR})
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../zlib)

#--------------------------------------------------------------------
# Add OpenEXR files

set(HALF_HEADER_FILES
    eLut.h
    half.h
    halfFunction.h
    halfLimits.h
    toFloat.h
)
set(HALF_SOURCE_FILES
    eLut.cpp
    half.cpp
    toFloat.cpp
)

file(GLOB_RECURSE IEX_HEADER_FILES ${CMAKE_CURRENT_SOURCE_DIR}/Ie*.h)
file(GLOB_RECURSE IEX_SOURCE_FILES ${CMAKE_CURRENT_SOURCE_DIR}/Ie*.cpp)

file(GLOB_RECURSE ILLMIMF_HEADER_FILES ${CMAKE_CURRENT_SOURCE_DIR}/Imf*.h)
file(GLOB_RECURSE ILLMIMF_SOURCE_FILES ${CMAKE_CURRENT_SOURCE_DIR}/Imf*.cpp)
list(APPEND ILLMIMF_HEADER_FILES ${CMAKE_CURRENT_SOURCE_DIR}/b44ExpLogTable.h)

set(ILMTHREAD_HEADER_FILES
    IlmThread.h
    IlmThreadMutex.h
    IlmThreadPool.h
    IlmThreadSemaphore.h
)
if(WIN32)
set(ILMTHREAD_SOURCE_FILES
    IlmThreadPool.cpp
    IlmThreadPosix.cpp
    IlmThreadPool.cpp
    IlmThreadMutexWin32.cpp
    IlmThreadSemaphoreWin32.cpp
    IlmThreadWin32.cpp
)
else()
set(ILMTHREAD_SOURCE_FILES
    IlmThread.cpp
    IlmThreadMutex.cpp
    IlmThreadMutexPosix.cpp
    IlmThreadPool.cpp
    IlmThreadPosix.cpp
    IlmThreadPool.cpp
    IlmThreadSemaphore.cpp
    IlmThreadSemaphorePosix.cpp
    IlmThreadSemaphorePosixCompat.cpp
)
endif()

file(GLOB_RECURSE IMATH_HEADER_FILES ${CMAKE_CURRENT_SOURCE_DIR}/Imath*.h)
file(GLOB_RECURSE IMATH_SOURCE_FILES ${CMAKE_CURRENT_SOURCE_DIR}/Imath*.cpp)

set(OPENEXR_HEADER_FILES ${HALF_HEADER_FILES} ${IEX_HEADER_FILES} ${ILLMIMF_HEADER_FILES} ${ILMTHREAD_HEADER_FILES} ${CMAKE_CURRENT_SOURCE_DIR}/IlmBaseConfig.h
    ${CMAKE_CURRENT_SOURCE_DIR}/OpenEXRConfig.h)
set(OPENEXR_SOURCE_FILES ${HALF_SOURCE_FILES} ${IEX_SOURCE_FILES} ${ILLMIMF_SOURCE_FILES} ${ILMTHREAD_SOURCE_FILES} ${CMAKE_CURRENT_SOURCE_DIR}/asd.cpp)

#--------------------------------------------------------------------
# Define defintions
if(BUILD_SHARED_LIBS)
    add_definitions(-DOPENEXR_DLL)
    add_definitions(-DOPENEXR_EXPORTS)
    add_definitions(-DZLIB_DLL)
	add_definitions(-DHALF_EXPORTS)
    add_definitions(-DIEX_EXPORTS)
	add_definitions(-DILMIMF_EXPORTS)
	add_definitions(-DILMTHREAD_EXPORTS)
	add_definitions(-DIMATH_EXPORTS)
endif()

#--------------------------------------------------------------------
# Create library
add_library(openexr ${OPENEXR_HEADER_FILES} ${OPENEXR_SOURCE_FILES})

#--------------------------------------------------------------------
# Add dependencies
add_dependencies(openexr zlib)

#--------------------------------------------------------------------
# Dependency libs
target_link_libraries(openexr zlib)