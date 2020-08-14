cmake_minimum_required(VERSION 2.8.3)
project(segmentation_pkg)

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

add_executable(region_growing_segmentation_ros
  src/region_growing_segmentation.cpp)
target_link_libraries(region_growing_segmentation_ros
  ${catkin_LIBRARIES}
  ${PCL_LIBRARIES})

add_executable(don_segmentation_ros
  src/don_segmentation.cpp)
target_link_libraries(don_segmentation_ros
  ${catkin_LIBRARIES}
  ${PCL_LIBRARIES})

add_executable(supervoxel_clustering_ros
  src/supervoxel_clustering.cpp)
target_link_libraries(supervoxel_clustering_ros
  ${catkin_LIBRARIES}
  ${PCL_LIBRARIES})

###########
# Install #
###########

set(EXEC_FILES
  region_growing_segmentation_ros
  don_segmentation_ros
  supervoxel_clustering_ros
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
