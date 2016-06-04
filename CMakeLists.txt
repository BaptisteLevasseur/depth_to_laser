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
  LIBRARIES DepthImageToLaserScanMod DepthImageToLaserScanROSMod
  CATKIN_DEPENDS dynamic_reconfigure image_geometry image_transport roscpp sensor_msgs
)

include_directories(include ${catkin_INCLUDE_DIRS})

add_library(DepthImageToLaserScanMod src/DepthImageToLaserScan.cpp)
target_link_libraries(DepthImageToLaserScanMod ${catkin_LIBRARIES})

add_library(DepthImageToLaserScanROSMod src/DepthImageToLaserScanROS.cpp)
add_dependencies(DepthImageToLaserScanROSMod ${PROJECT_NAME}_gencfg)
target_link_libraries(DepthImageToLaserScanROSMod DepthImageToLaserScanMod ${catkin_LIBRARIES})

add_executable(depthimage_to_laserscan src/depthimage_to_laserscan.cpp)
target_link_libraries(depthimage_to_laserscan DepthImageToLaserScanROSMod ${catkin_LIBRARIES})


# Install targets
install(TARGETS DepthImageToLaserScanMod DepthImageToLaserScanROSMod depthimage_to_laserscan
	RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
	LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
	ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION})

install(DIRECTORY include/${PROJECT_NAME}/
        DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
)
