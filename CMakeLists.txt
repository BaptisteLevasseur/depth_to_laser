# http://ros.org/doc/groovy/api/catkin/html/user_guide/supposed.html
cmake_minimum_required(VERSION 2.8)
project(depth_to_laser)

# Load catkin and all dependencies required for this package
find_package(catkin REQUIRED dynamic_reconfigure image_geometry image_transport nodelet roscpp sensor_msgs)
#find_package(OpenCV REQUIRED)

# Dynamic reconfigure support
generate_dynamic_reconfigure_options(cfg/Depth.cfg)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES DepthImageToLaserScan DepthImageToLaserScanROS 
  CATKIN_DEPENDS dynamic_reconfigure image_geometry image_transport roscpp sensor_msgs
)

include_directories(include ${catkin_INCLUDE_DIRS})

add_library(DepthImageToLaserScan src/DepthImageToLaserScan.cpp)
target_link_libraries(DepthImageToLaserScan ${catkin_LIBRARIES})

add_library(DepthImageToLaserScanROS src/DepthImageToLaserScanROS.cpp)
add_dependencies(DepthImageToLaserScanROS ${PROJECT_NAME}_gencfg)
target_link_libraries(DepthImageToLaserScanROS DepthImageToLaserScan ${catkin_LIBRARIES})

add_executable(depthimage_to_laserscan src/depthimage_to_laserscan.cpp)
target_link_libraries(depthimage_to_laserscan DepthImageToLaserScanROS ${catkin_LIBRARIES})


# Install targets
install(TARGETS DepthImageToLaserScan DepthImageToLaserScanROS depthimage_to_laserscan
	RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
	LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
	ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION})

install(DIRECTORY include/${PROJECT_NAME}/
        DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
)
