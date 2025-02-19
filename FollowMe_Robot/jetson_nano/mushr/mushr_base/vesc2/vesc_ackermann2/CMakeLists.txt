cmake_minimum_required(VERSION 2.8.3)
project(vesc_ackermann2)

find_package(catkin REQUIRED COMPONENTS
  nodelet
  pluginlib
  roscpp
  nav_msgs
  std_msgs
  geometry_msgs
  tf
  ackermann_msgs
  vesc_msgs
)

catkin_package(
  INCLUDE_DIRS include
  CATKIN_DEPENDS
    nodelet
    pluginlib
    roscpp
    nav_msgs
    std_msgs
    geometry_msgs
    tf
    ackermann_msgs
    vesc_msgs
)

###########
## Build ##
###########

include_directories(
  include
  ${Boost_INCLUDE_DIRS}
  ${catkin_INCLUDE_DIRS}
)

# node executable
add_executable(vesc_to_odom_node2 src/vesc_to_odom_node2.cpp
                                 src/vesc_to_odom.cpp)
add_dependencies(vesc_to_odom_node2 ${catkin_EXPORTED_TARGETS})
target_link_libraries(vesc_to_odom_node2
  ${catkin_LIBRARIES}
)

add_executable(ackermann_to_vesc_node2 src/ackermann_to_vesc_node2.cpp
                                      src/ackermann_to_vesc.cpp)
add_dependencies(ackermann_to_vesc_node2 ${catkin_EXPORTED_TARGETS})
target_link_libraries(ackermann_to_vesc_node2
  ${catkin_LIBRARIES}
)

# nodelet library
add_library(vesc_ackermann_nodelet2 src/ackermann_to_vesc_nodelet.cpp
                                   src/ackermann_to_vesc.cpp
                                   src/vesc_to_odom_nodelet.cpp
                                   src/vesc_to_odom.cpp)
add_dependencies(vesc_ackermann_nodelet2 ${catkin_EXPORTED_TARGETS})
target_link_libraries(vesc_ackermann_nodelet2
  ${catkin_LIBRARIES}
)

#############
## Install ##
#############

install(TARGETS vesc_to_odom_node2 ackermann_to_vesc_node2
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(TARGETS vesc_ackermann_nodelet2
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
)

install(DIRECTORY include/${PROJECT_NAME}/
        DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION})
install(FILES vesc_ackermann_nodelet.xml
        DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})
install(DIRECTORY launch/
        DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/launch)

#############
## Testing ##
#############

# TODO

