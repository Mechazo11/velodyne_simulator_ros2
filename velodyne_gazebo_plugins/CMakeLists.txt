cmake_minimum_required(VERSION 3.8)

project(velodyne_gazebo_plugins)

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()
if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wextra -Wpedantic")
endif()

# Dependencies updated to be compatible with Gazebo Harmonic
find_package(ament_cmake REQUIRED)
find_package(gz-cmake3 REQUIRED)
find_package(gz-msgs10 REQUIRED)
find_package(gz-sim8 REQUIRED)
find_package(ros_gz_bridge REQUIRED)
find_package(ros_gz_sim REQUIRED)
find_package(rclcpp REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(tf2 REQUIRED)

#include_directories(include)

link_directories(
  # ${GAZEBO_LIBRARY_DIRS}
)

include_directories(
  include
  # ${GAZEBO_INCLUDE_DIRS}
)

add_library(gazebo_ros_velodyne_laser SHARED src/GazeboRosVelodyneLaser.cpp)
ament_target_dependencies(gazebo_ros_velodyne_laser
  "gz-cmake3"
  "gz-msgs10"
  "gz-sim8"
  "gz-sensors8"
  "ros_gz_bridge"
  "ros_gz_sim"
  "rclcpp"
  "sensor_msgs"
)
ament_export_libraries(gazebo_ros_velodyne_laser)

ament_export_include_directories(include)
ament_export_dependencies(rclcpp)
ament_export_dependencies(gz-cmake3)
ament_export_dependencies(gz-msgs10)
ament_export_dependencies(gz-sim8)
ament_export_dependencies(gz-sensors8)
ament_export_dependencies(ros_gz_bridge)
ament_export_dependencies(ros_gz_sim)
ament_export_dependencies(sensor_msgs)
ament_export_dependencies(tf2)

ament_package()

install(DIRECTORY include/
        DESTINATION include)

install(TARGETS
    gazebo_ros_velodyne_laser
    ARCHIVE DESTINATION lib
    LIBRARY DESTINATION lib
    RUNTIME DESTINATION lib/${PROJECT_NAME}
)
