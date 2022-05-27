cmake_minimum_required(VERSION 2.8.3)
project(nui_coex_auv_2022)

find_package(catkin REQUIRED
  #COMPONENTS
        #rospy
  )


catkin_package(
#  INCLUDE_DIRS include
#  LIBRARIES
#  CATKIN_DEPENDS
#  DEPENDS system_lib
)

install(FILES
    launch/demo_coex.launch
    DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})

