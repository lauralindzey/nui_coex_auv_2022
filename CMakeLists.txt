cmake_minimum_required(VERSION 2.8.3)
project(nui_config)

add_compile_options(-std=c++11)

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
    launch/nui.launch
    DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})

