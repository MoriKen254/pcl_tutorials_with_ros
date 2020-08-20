cmake_minimum_required(VERSION 2.8.3)
project(filtering_pkg)

add_compile_options(-std=c++14)

find_package(catkin REQUIRED COMPONENTS
  pcl_conversions
  pcl_msgs
  pcl_ros
  roscpp
  rospy
  sensor_msgs
  std_msgs
  tf
)

find_package(PCL REQUIRED)

catkin_package(
  INCLUDE_DIRS include
  CATKIN_DEPENDS
    pcl_conversions
    pcl_msgs
    pcl_ros
    roscpp
    rospy
    sensor_msgs
    std_msgs
    tf
  DEPENDS PCL
)

include_directories(
  include
  ${catkin_INCLUDE_DIRS}
  ${PCL_INCLUDE_DIRS}
)

###########
## Build ##
###########

add_executable(passthrough_ros
  src/passthrough.cpp)
target_link_libraries(passthrough_ros
  ${catkin_LIBRARIES}
  ${PCL_LIBRARIES})

add_executable(voxel_grid_ros
  src/voxel_grid.cpp)
target_link_libraries(voxel_grid_ros
  ${catkin_LIBRARIES}
  ${PCL_LIBRARIES})

add_executable(statistical_removal_ros
  src/statistical_removal.cpp)
target_link_libraries(statistical_removal_ros
  ${catkin_LIBRARIES}
  ${PCL_LIBRARIES})

add_executable(project_inliers_ros
  src/project_inliers.cpp)
target_link_libraries(project_inliers_ros
  ${catkin_LIBRARIES}
  ${PCL_LIBRARIES})

add_executable(extract_indices_ros
  src/extract_indices.cpp)
target_link_libraries(extract_indices_ros
  ${catkin_LIBRARIES}
  ${PCL_LIBRARIES})

add_executable(remove_outliers_ros
  src/remove_outliers.cpp)
target_link_libraries(remove_outliers_ros
  ${catkin_LIBRARIES}
  ${PCL_LIBRARIES})

###########
# Install #
###########

set(EXEC_FILES
  passthrough_ros
)

# Install executables
foreach(nodes ${EXEC_FILES})
  install(TARGETS ${nodes}
    RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})
endforeach()

# Install include directory
install(DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
)

# Install all files
set(DIRECTORIES
  launch
)

foreach(dirs ${DIRECTORIES})
  install(DIRECTORY ${dirs}/
    DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/${dirs})
endforeach()


#############
## Testing ##
#############

if (CATKIN_ENABLE_TESTING)
  find_package(roslaunch REQUIRED)

  set(DIRECTORIES
    launch
  )

  foreach(dirs ${DIRECTORIES})
    roslaunch_add_file_check(${dirs})
  endforeach()
endif()
