cmake_minimum_required(VERSION 3.8)
project(rfid_sensor)

if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 17)
endif()
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(gazebo_ros REQUIRED)
find_package(gazebo REQUIRED)
find_package(std_msgs REQUIRED)
find_package(geometry_msgs REQUIRED)
find_package(rosidl_default_generators REQUIRED)

rosidl_generate_interfaces(${PROJECT_NAME}
  "msg/Tag.msg"
  "msg/TagArray.msg"
  DEPENDENCIES std_msgs geometry_msgs
)
rosidl_get_typesupport_target(cpp_typesupport_target
  ${PROJECT_NAME} rosidl_typesupport_cpp)

add_library(gazeboRfidAntennaPlugin SHARED src/rfidAntenna.cpp)
add_library(gazeboRfidTagPlugin SHARED src/rfidTag.cpp)
separate_arguments(gazebo_cxx_flags NATIVE_COMMAND "${GAZEBO_CXX_FLAGS}")
foreach(target gazeboRfidAntennaPlugin gazeboRfidTagPlugin)
  target_include_directories(${target} PRIVATE include ${GAZEBO_INCLUDE_DIRS})
  target_link_directories(${target} PRIVATE ${GAZEBO_LIBRARY_DIRS})
  target_link_libraries(${target} ${GAZEBO_LIBRARIES})
  target_compile_options(${target} PRIVATE ${gazebo_cxx_flags})
endforeach()
ament_target_dependencies(gazeboRfidAntennaPlugin rclcpp gazebo_ros)
target_link_libraries(gazeboRfidAntennaPlugin "${cpp_typesupport_target}")

install(TARGETS gazeboRfidAntennaPlugin gazeboRfidTagPlugin
  LIBRARY DESTINATION lib)
ament_export_dependencies(rosidl_default_runtime)
ament_package()
