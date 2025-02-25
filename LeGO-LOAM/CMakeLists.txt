cmake_minimum_required(VERSION 3.0.2)
project(lego_loam_bor)

set(CMAKE_BUILD_TYPE "Release")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -g -std=c++14 -ffast-math -DGTSAM_USE_SYSTEM_EIGEN=ON")


#

find_package(catkin REQUIRED COMPONENTS
  tf
  roscpp
  rospy
  image_transport
  rosbag_storage

  pcl_ros
  pcl_conversions

  std_msgs
  sensor_msgs
  geometry_msgs
  nav_msgs
  cloud_msgs
)

find_package(GTSAM REQUIRED QUIET)
find_package(PCL REQUIRED QUIET)
find_package(Boost REQUIRED QUIET COMPONENTS serialization thread timer chrono)

catkin_package(
  INCLUDE_DIRS include
  CATKIN_DEPENDS cloud_msgs rosbag_storage
  DEPENDS PCL
)

include_directories(
	include
	${catkin_INCLUDE_DIRS}
	${PCL_INCLUDE_DIRS}
	${GTSAM_INCLUDE_DIR}
)

link_directories(
	include
	${PCL_LIBRARY_DIRS}
	${GTSAM_LIBRARY_DIRS}
)

add_executable(lego_loam_bor
    src/imageProjection.cpp
    src/featureAssociation.cpp
    src/mapOptmization.cpp
    src/transformFusion.cpp
    src/main.cpp)

add_dependencies(lego_loam_bor ${catkin_EXPORTED_TARGETS} cloud_msgs_generate_messages_cpp)
target_link_libraries(lego_loam_bor ${catkin_LIBRARIES} ${PCL_LIBRARIES} gtsam)

