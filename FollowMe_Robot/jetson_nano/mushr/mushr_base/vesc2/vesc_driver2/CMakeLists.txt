cmake_minimum_required(VERSION 2.8.3)
project(vesc_driver2)

find_package(catkin REQUIRED COMPONENTS
  nodelet
  pluginlib
  roscpp
  std_msgs
  vesc_msgs
  serial
)

catkin_package(
  INCLUDE_DIRS include
  CATKIN_DEPENDS nodelet pluginlib roscpp std_msgs vesc_msgs serial
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
add_executable(vesc_driver_node2 src/vesc_driver_node2.cpp
                                src/vesc_driver.cpp
                                src/vesc_interface.cpp
                                src/vesc_packet.cpp
                                src/vesc_packet_factory.cpp)
add_dependencies(vesc_driver_node2 ${catkin_EXPORTED_TARGETS})
target_link_libraries(vesc_driver_node2
  ${catkin_LIBRARIES}
)

# nodelet library
add_library(vesc_driver_nodelet2 src/vesc_driver_nodelet.cpp
                                src/vesc_driver.cpp
                                src/vesc_interface.cpp
                                src/vesc_packet.cpp
                                src/vesc_packet_factory.cpp)
add_dependencies(vesc_driver_nodelet2 ${catkin_EXPORTED_TARGETS})
target_link_libraries(vesc_driver_nodelet2
  ${catkin_LIBRARIES}
)

#############
## Install ##
#############

install(TARGETS vesc_driver_node2
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
install(TARGETS vesc_driver_nodelet2
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
)

install(DIRECTORY include/${PROJECT_NAME}/
        DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION})
install(FILES vesc_driver_nodelet.xml
        DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})
install(DIRECTORY launch/
        DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/launch)

#############
## Testing ##
#############

# TODO

