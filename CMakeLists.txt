
###################################################################################
#
#  Copyright (c) 2014, webvariants GmbH, http://www.webvariants.de
#
#  This file is released under the terms of the MIT license. You can find the
#  complete text in the attached LICENSE file or online at:
#
#  http://www.opensource.org/licenses/mit-license.php
#
#  @author: Tino Rusch (tino.rusch@webvariants.de)
#
###################################################################################

cmake_minimum_required(VERSION 2.8 FATAL_ERROR)

project(bcrypt)

enable_language(ASM)

set( CMAKE_COLOR_MAKEFILE ON )

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)

set( CMAKE_CXX_FLAGS  "${CMAKE_CXX_FLAGS} -Wall --std=c++11 -O3" )
set( CMAKE_C_FLAGS  "${CMAKE_C_FLAGS} -Wall -O3" )

set( CMAKE_ASM_FLAGS "${CXXFLAGS} -x assembler-with-cpp")

set( SRCFILES ${CMAKE_CURRENT_SOURCE_DIR}/src/bcrypt.c
		${CMAKE_CURRENT_SOURCE_DIR}/src/crypt_blowfish.c
		${CMAKE_CURRENT_SOURCE_DIR}/src/crypt_gensalt.c
		${CMAKE_CURRENT_SOURCE_DIR}/src/wrapper.c
		${CMAKE_CURRENT_SOURCE_DIR}/src/x86.S
)

include_directories( ${CMAKE_CURRENT_SOURCE_DIR}/include/bcrypt)
include_directories( ${CMAKE_CURRENT_SOURCE_DIR}/include)

#add_library( ${PROJECT_NAME} SHARED ${SRCFILES})
add_library( ${PROJECT_NAME} STATIC ${SRCFILES})

add_executable( ${PROJECT_NAME}_test ${CMAKE_CURRENT_SOURCE_DIR}/src/main.cpp)
target_link_libraries( ${PROJECT_NAME}_test ${PROJECT_NAME})

install(TARGETS ${PROJECT_NAME}
  RUNTIME DESTINATION lib
  LIBRARY DESTINATION lib
  ARCHIVE DESTINATION lib)

install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/ DESTINATION include)

SET(CPACK_GENERATOR "DEB")
SET(CPACK_SET_DESTDIR ON)
SET(CPACK_DEBIAN_PACKAGE_MAINTAINER "Tino Rusch")
SET(CPACK_PACKAGE_VERSION "0.1.0")
SET(CPACK_PACKAGE_VERSION_MAJOR "0")
SET(CPACK_PACKAGE_VERSION_MINOR "1")
SET(CPACK_PACKAGE_VERSION_PATCH "0")
INCLUDE(CPack)
