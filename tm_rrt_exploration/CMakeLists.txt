cmake_minimum_required(VERSION 3.8)
project(tmrrt_exploration)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# necessary to get past eigen resizing (?)
set(CMAKE_BUILD_TYPE RelWithDebInfo) #None, Debug, Release, RelWithDebInfo, MinSizeRel

find_package(ament_cmake REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(nav_msgs REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rclpy REQUIRED)
find_package(std_msgs REQUIRED)
find_package(tf2 REQUIRED)
find_package(tf2_ros REQUIRED)
find_package(visualization_msgs REQUIRED)
find_package(rosidl_default_generators REQUIRED)

rosidl_generate_interfaces(${PROJECT_NAME}
  "msg/PointArray.msg"
  "msg/InvalidArray.msg"
  DEPENDENCIES sensor_msgs geometry_msgs
)

ament_export_dependencies(rosidl_default_runtime)

ament_package()

# include_directories(include ${catkin_INCLUDE_DIRS})

set(TARGET global_rrt_detector)
set(SOURCES
  src/global_rrt_detector.cpp
  src/functions.cpp
  src/mtrand.cpp
)
add_executable(${TARGET} ${SOURCES})
target_include_directories(${TARGET} PUBLIC include)
target_link_libraries(
  ${TARGET}
  "${cpp_typesupport_target}"
)
ament_target_dependencies(${TARGET}
  rclcpp
  tf2
  tf2_ros
  geometry_msgs
  sensor_msgs
  nav_msgs
  visualization_msgs
)

# add_executable(global_rrt_detector src/global_rrt_detector.cpp src/functions.cpp src/mtrand.cpp)
# target_link_libraries(global_rrt_detector ${catkin_LIBRARIES})

# add_executable(local_rrt_detector src/local_rrt_detector.cpp src/functions.cpp src/mtrand.cpp)
# target_link_libraries(local_rrt_detector ${catkin_LIBRARIES})
