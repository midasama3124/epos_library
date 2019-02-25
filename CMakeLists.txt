cmake_minimum_required(VERSION 2.8.3)
project(epos_library)

find_package(catkin REQUIRED)

# find linux architecture
EXECUTE_PROCESS( COMMAND arch OUTPUT_VARIABLE ARCHITECTURE OUTPUT_STRIP_TRAILING_WHITESPACE )

# find the 32/64 bits or rpi libraries
#if(CMAKE_SIZEOF_VOID_P EQUAL 8)
if(${ARCHITECTURE} STREQUAL "x86_64")
  # Architecture for 64-bits distribution
  set(ARCH "intel/x86_64")
elseif(${ARCHITECTURE} STREQUAL "armv7l")
  # Architecture for Raspberry Pi3 Model B
  set(ARCH "arm/v7")
else()
  # Architecture for 32-bits distribution
  set(ARCH "intel/x86")
endif()

message(STATUS "Detected architecture: ${ARCHITECTURE}")

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES ftd2xx EposCmd
)

###########
## Build ##
###########

include_directories(include)

# Set library path
set(ftd2xx_LIBRARY ${PROJECT_SOURCE_DIR}/lib/${ARCH}/libftd2xx.so.1.4.8)
set(EposCmd_LIBRARY ${PROJECT_SOURCE_DIR}/lib/${ARCH}/libEposCmd.so.6.5.1.0)

add_library(ftd2xx SHARED ${ftd2xx_LIBRARY})
add_custom_command(TARGET ftd2xx POST_BUILD COMMAND cp ${ftd2xx_LIBRARY} ${CMAKE_LIBRARY_OUTPUT_DIRECTORY}/libftd2xx.so)
set_target_properties(ftd2xx PROPERTIES LINKER_LANGUAGE CXX)

add_library(EposCmd SHARED ${EposCmd_LIBRARY})
add_custom_command(TARGET EposCmd POST_BUILD COMMAND cp ${EposCmd_LIBRARY} ${CMAKE_LIBRARY_OUTPUT_DIRECTORY}/libEposCmd.so)
set_target_properties(EposCmd PROPERTIES LINKER_LANGUAGE CXX)

#############
## Install ##
#############

install(TARGETS ftd2xx EposCmd
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
  FILES_MATCHING PATTERN "*.h"
  PATTERN ".svn" EXCLUDE
)
