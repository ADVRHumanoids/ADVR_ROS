 #
#  Copyright (C) 2017 IIT-ADVR
#  Author: Luca Muratore
#  email: luca.muratore@iit.it
#
#  This program is free software: you can redistribute it and/or modify
#  it under the terms of the GNU Lesser General Public License as published by
#  the Free Software Foundation, either version 3 of the License, or
#  (at your option) any later version.
#
#  This program is distributed in the hope that it will be useful,
#  but WITHOUT ANY WARRANTY; without even the implied warranty of
#  MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE. See the
#  GNU Lesser General Public License for more details.
#
#  You should have received a copy of the GNU Lesser General Public License
#  along with this program. If not, see <http://www.gnu.org/licenses/>
#

cmake_minimum_required(VERSION 2.8.12)

include(ExternalProject)
project(ADVR_ROS)

# C++ 11
include(CheckCXXCompilerFlag)
check_cxx_compiler_flag("-std=c++11" COMPILER_SUPPORTS_CXX11)
check_cxx_compiler_flag("-std=c++0x" COMPILER_SUPPORTS_CXX0X)
if(COMPILER_SUPPORTS_CXX11)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
elseif(COMPILER_SUPPORTS_CXX0X)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x")
else()
    message(FATAL_ERROR "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")
endif()

# Add cmake dir and include custom macros
list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/cmake")
include(MacroYCMInstallLibrary)

# find first YCM in order to have ${YCM_MODULE_PATH} defined for 3rd party software like Eigen3
find_package(YCM REQUIRED)

# ROS
find_package(catkin QUIET COMPONENTS roscpp 
                                     sensor_msgs 
                                     std_srvs 
                                     message_generation 
                                     robot_state_publisher
                                     trajectory_utils
                                     )



# # include
# include_directories(include)
# 
# # for every file in INCLUDES CMake already sets the property HEADER_FILE_ONLY
# file(GLOB_RECURSE ADVR_ROS_INCLUDES "${CMAKE_CURRENT_SOURCE_DIR}/include" *.h*)

# generate ROS messages and services
if( ${catkin_FOUND} )

    add_service_files(
    DIRECTORY srv
    FILES
    advr_cartesian_control.srv
    )
    
    add_service_files(
    DIRECTORY srv
    FILES
    advr_segment_control.srv
    )
    
    add_service_files(
    DIRECTORY srv
    FILES
    im_pose.srv
    )

    add_service_files(
    DIRECTORY srv
    FILES
    im_pose_array.srv
    )
    
    add_service_files(
    DIRECTORY srv
    FILES
    advr_grasp_control.srv
    )
    
    add_service_files(
    DIRECTORY srv
    FILES
    advr_grasp_control_srv.srv
    )
    
    add_service_files(
    DIRECTORY srv
    FILES
    advr_locomotion.srv
    )

    add_message_files(
    DIRECTORY msgs
    FILES
    im_pose_msg.msg
    im_pose_array_msg.msg
    tactile_sensor.msg
    )
        
    generate_messages(
    DEPENDENCIES
    std_msgs
    geometry_msgs
    trajectory_utils
    )

catkin_package()



endif()

#install
# ycm_library_install("msg" 1 0 0)
