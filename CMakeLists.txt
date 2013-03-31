#//////////////////////////////////////////////////////////////////////////////
#
#  Package:  unco-heliostat
#  
# /*! \file
#  *  
#  *  \brief Top-level CMake config file for the 'heliostat' package. 
#  *
#  *  \author Daniel Packard (pack3754@gmail.com)
#  *  
#  *  \par Copyright:
#  *  (C) 2013, Daniel Packard
#  *  (http://pack3754.github.com/unco-heliostat)
#  *  All rights reserved.
#  */
#
# Redistribution and use in source and binary forms, with or without
# modification, are permitted provided that the following conditions are met: 
#
# 1. Redistributions of source code must retain the above copyright notice, 
#    this list of conditions and the following disclaimer.
#
# 2. Redistributions in binary form must reproduce the above copyright notice,
#    this list of conditions and the following disclaimer in the documentation
#    and/or other materials provided with the distribution. 
#
# THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS" 
# AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE 
# IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE 
# ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT OWNER OR CONTRIBUTORS BE 
# LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR 
# CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF 
# SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS 
# INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN 
# CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE) 
# ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE 
# POSSIBILITY OF SUCH DAMAGE.
#
# The views and conclusions contained in the software and documentation are 
# those of the authors and should not be interpreted as representing official 
# policies, either expressed or implied, of the FreeBSD Project.

cmake_minimum_required (VERSION 2.6)

#
# project name
#
project (heliostat)

#
# project version
#
set (pkg_version_major    0)
set (pkg_version_minor    0)
set (pkg_version_revision 0)

#
# project include dirs
#
include_directories ("${PROJECT_SOURCE_DIR}/include")
include_directories ("${PROJECT_BINARY_DIR}/include")
include_directories ("/opt/include")

#
# project version.h 
#
configure_file (
  "${PROJECT_SOURCE_DIR}/include/version.h.in"
  "${PROJECT_BINARY_DIR}/include/version.h"
)

#
# project lib directories
#
link_directories ("/opt/lib")

#
# distribute shared libraries
#
set (BUILD_SHARED_LIBS ON)

#
# unit tests
#
option (build_tests "Build heliostat unit tests." OFF)

if(build_tests)
  enable_testing()
endif()

#
# enable doxygen source documentation (`make doc`)
#
find_package(Doxygen)

if(DOXYGEN_FOUND)

  configure_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/config/doxy.in 
    ${CMAKE_CURRENT_BINARY_DIR}/doxy.conf @ONLY
  )

  add_custom_target(doc
    ${DOXYGEN_EXECUTABLE} 
    ${CMAKE_CURRENT_BINARY_DIR}/doxy.conf
    WORKING_DIRECTORY 
    ${CMAKE_CURRENT_BINARY_DIR}
    COMMENT "Generating API documentation with Doxygen" VERBATIM
  )

endif(DOXYGEN_FOUND)

#
# add project subdirectories
#
add_subdirectory (sw)
