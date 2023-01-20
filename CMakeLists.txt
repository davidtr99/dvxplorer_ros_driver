cmake_minimum_required(VERSION 3.0.2)
project(dvxplorer_ros_driver)

# ==== Build type ====
SET(CMAKE_CONFIGURATION_TYPES "Debug;Release" CACHE STRING "Configs" FORCE)

IF(DEFINED CMAKE_BUILD_TYPE)
    SET_PROPERTY(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS ${CMAKE_CONFIGURATION_TYPES})
ENDIF()

IF(NOT CMAKE_BUILD_TYPE)
    SET(CMAKE_BUILD_TYPE "Release")
ENDIF()

IF(NOT ${CMAKE_BUILD_TYPE} STREQUAL "Debug" AND
    NOT ${CMAKE_BUILD_TYPE} STREQUAL "Release")
    MESSAGE(FATAL_ERROR "Only Release and Debug build types are allowed.")
ENDIF()

# ==== Depdendencies ====
find_package(catkin REQUIRED COMPONENTS
  roscpp
  nodelet
  dvs_msgs
  dynamic_reconfigure
  camera_info_manager
)

find_package(Boost REQUIRED COMPONENTS system thread)
find_package(libcaer REQUIRED)
 
SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++14 -O3")

#########################

## Generate dynamic reconfigure parameters in the 'cfg' folder
generate_dynamic_reconfigure_options(
  cfg/DVXplorer_ROS_Driver.cfg
)

include_directories(
    include
    ${catkin_INCLUDE_DIRS}
)

link_directories(
    include
)


file(GLOB_RECURSE SRC_INCS "include/${PROJECT_NAME}/*.h*")
file(GLOB_RECURSE SRC_SRCS "src/*.c*")
file(GLOB_RECURSE SRC_HDRS "src/*.h*")


add_library(${PROJECT_NAME} ${SRC_SRCS} ${SRC_HDRS} ${SRC_INCS} utils/driver_nodelet.cpp)
add_dependencies(${PROJECT_NAME} ${catkin_EXPORTED_TARGETS})
target_link_libraries(${PROJECT_NAME} ${catkin_LIBRARIES} ${Boost_LIBRARIES} libcaer::caer)

add_executable(${PROJECT_NAME}_node utils/driver_node.cpp)
target_link_libraries(${PROJECT_NAME}_node ${PROJECT_NAME})


# find_package(catkin_simple REQUIRED)
# find_package(libcaer REQUIRED)

# catkin_simple()


# find_package(Boost REQUIRED COMPONENTS system thread)

# # make the executable
# cs_add_executable(dvxplorer_ros_driver
#   src/driver_node.cpp
#   src/driver.cpp
# )

# # make the nodelet into a library
# cs_add_library(dvxplorer_ros_driver_nodelet
#   src/driver_nodelet.cpp
#   src/driver.cpp
# )

# # link the executable to the necessary libs
# target_link_libraries(dvxplorer_ros_driver
#   ${catkin_LIBRARIES}
#   ${Boost_LIBRARIES}
#   libcaer::caer
# )

# # link the executable to the necessary libs
# target_link_libraries(dvxplorer_ros_driver_nodelet
#   ${catkin_LIBRARIES}
#   ${Boost_LIBRARIES}
#   libcaer::caer
# )

# # Install the nodelet library
# cs_install()

# # Install other support files for installation
# install(FILES dvxplorer_ros_driver_nodelet.xml
#   DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
# )

# # Export package
# cs_export()
