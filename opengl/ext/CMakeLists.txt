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

option(BUILD_GLEW "Build GLEW" ON)
option(BUILD_GLFW "Build GLFW" ON)

mark_as_advanced(USE_MSVC_RUNTIME_LIBRARY_DLL)
mark_as_advanced(LIB_SUFFIX)

#--------------------------------------------------------------------
# Build GLEW and GLFW
if(BUILD_GLEW AND (EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/glew))
    add_subdirectory(glew)
endif()

if(BUILD_GLFW AND (EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/glfw))
    add_subdirectory(glfw)
	
	set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "Build the GLFW example programs" FORCE)
	set(GLFW_BUILD_TESTS OFF CACHE BOOL "Build the GLFW test programs" FORCE)
	set(GLFW_BUILD_DOCS OFF CACHE BOOL "Build the GLFW documentation" FORCE)
	set(GLFW_INSTALL OFF CACHE BOOL "Generate installation target" FORCE)
	set(GLFW_DOCUMENT_INTERNALS OFF CACHE BOOL "Include internals in documentation" FORCE)

	mark_as_advanced(GLFW_BUILD_EXAMPLES)
	mark_as_advanced(GLFW_BUILD_TESTS)
	mark_as_advanced(GLFW_BUILD_DOCS)
	mark_as_advanced(GLFW_INSTALL)
	mark_as_advanced(GLFW_DOCUMENT_INTERNALS)
	mark_as_advanced(GLFW_DIR)
	mark_as_advanced(GLFW_LIBRARIES)
	mark_as_advanced(GLFW_USE_DWM_SWAP_INTERVAL)
	mark_as_advanced(GLFW_USE_EGL)
	mark_as_advanced(GLFW_USE_OPTIMUS_HPG)
endif()