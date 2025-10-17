cmake_minimum_required(VERSION 3.21)
project(rgl_gazebo_plugin)

# ament cmake required for setting up proper environment hooks when building in a colcon workspace
find_package(ament_cmake REQUIRED)

if (CMAKE_INSTALL_PREFIX_INITIALIZED_TO_DEFAULT)
    set(CMAKE_INSTALL_PREFIX "${PROJECT_SOURCE_DIR}/install")
endif()

add_subdirectory(external)
add_subdirectory(RGLServerPlugin)
add_subdirectory(RGLVisualize)

# Use ament_environment hooks to update the necessary environment variables automatically when
# sourcing the workspace where this package is built. Variables updated:
# - GZ_SIM_SYSTEM_PLUGIN_PATH
# - GZ_GUI_PLUGIN_PATH
# - GZ_SIM_RESOURCE_PATH
# - RGL_PATTERNS_DIR
ament_environment_hooks("${CMAKE_CURRENT_SOURCE_DIR}/hooks/${PROJECT_NAME}.dsv.in")
ament_environment_hooks("${CMAKE_CURRENT_SOURCE_DIR}/hooks/${PROJECT_NAME}.sh.in")

# Install test_world and lidar_patterns folders so that we can find them automatically in a ros2
# workspace using environment hooks
install(
  DIRECTORY test_world lidar_patterns
  DESTINATION share/${PROJECT_NAME}
)

ament_package()
