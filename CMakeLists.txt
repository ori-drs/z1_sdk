cmake_minimum_required(VERSION 3.0)
project(z1_sdk)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3 -std=c++14 -pthread")

include_directories(
  include
  /usr/include/eigen3
)

link_directories(lib)

set(EXAMPLES_FILES
    examples/highcmd_basic.cpp
    examples/highcmd_development.cpp
    examples/lowcmd_development.cpp
    examples/lowcmd_multirobots.cpp
)

find_package(catkin QUIET)
if(catkin_FOUND)
  # Catkin build (ROS)
  include_directories(
    ${catkin_INCLUDE_DIRS}
  )

  # Need to add Z1_SDK_${CMAKE_SYSTEM_PROCESSOR} as a shared library to be accessible to other packages
  add_library(Z1_SDK_${CMAKE_SYSTEM_PROCESSOR} SHARED IMPORTED)
  set_target_properties(Z1_SDK_${CMAKE_SYSTEM_PROCESSOR} PROPERTIES
    IMPORTED_LOCATION ${PROJECT_SOURCE_DIR}/lib/libZ1_SDK_${CMAKE_SYSTEM_PROCESSOR}.so
  )

  catkin_package(
    INCLUDE_DIRS include
    LIBRARIES Z1_SDK_${CMAKE_SYSTEM_PROCESSOR}
  )

  foreach(EXAMPLE_FILE IN LISTS EXAMPLES_FILES)
    get_filename_component(EXAMPLE_NAME ${EXAMPLE_FILE} NAME_WE)
    add_executable(${EXAMPLE_NAME} ${EXAMPLE_FILE})
    target_link_libraries(${EXAMPLE_NAME} ${catkin_LIBRARIES} Z1_SDK_${CMAKE_SYSTEM_PROCESSOR})
  endforeach()

  if(${pybind11_FOUND})
    pybind11_add_module(unitree_arm_interface examples_py/arm_python_interface.cpp)
    target_link_libraries(unitree_arm_interface PRIVATE ${catkin_LIBRARIES} Z1_SDK_${CMAKE_SYSTEM_PROCESSOR})
  endif()
else()
  # Cmake build (non-ROS)
  foreach(EXAMPLE_FILE IN LISTS EXAMPLES_FILES)
  get_filename_component(EXAMPLE_NAME ${EXAMPLE_FILE} NAME_WE)
  add_executable(${EXAMPLE_NAME} ${EXAMPLE_FILE})
  target_link_libraries(${EXAMPLE_NAME} Z1_SDK_${CMAKE_SYSTEM_PROCESSOR})
  endforeach()


  set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/lib)
  find_package(pybind11 QUIET)
  if(${pybind11_FOUND})
  pybind11_add_module(unitree_arm_interface examples_py/arm_python_interface.cpp)
  target_link_libraries(unitree_arm_interface PRIVATE Z1_SDK_${CMAKE_SYSTEM_PROCESSOR})
  endif()
endif()