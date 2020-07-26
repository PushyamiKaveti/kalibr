cmake_minimum_required(VERSION 2.8.3)
project(numpy_eigen)

find_package(catkin REQUIRED COMPONENTS cmake_modules python_module)
include_directories(${catkin_INCLUDE_DIRS})
find_package(Boost REQUIRED COMPONENTS system)
find_package(Eigen REQUIRED)

catkin_python_setup()

find_package(PythonLibs REQUIRED)
set(PYTHON_INCLUDE_DIRS ${PYTHON_INCLUDE_DIRS} /usr/local/lib/python2.7/dist-packages/numpy/core/include)
add_definitions(${EIGEN_DEFINITIONS})
include_directories(include ${Eigen_INCLUDE_DIRS} ${catkin_INCLUDE_DIRS} ${PYTHON_INCLUDE_DIRS}  /usr/include/python2.7/)
include_directories("${PROJECT_SOURCE_DIR}/include/numpy_eigen")

catkin_package(
  INCLUDE_DIRS include  ${catkin_INCLUDE_DIRS}
  LIBRARIES ${PROJECT_NAME}
  DEPENDS
)
add_definitions(-std=c++0x -D__STRICT_ANSI__)

include_directories(include ${Boost_INCLUDE_DIRS})

IF(APPLE)
  link_directories( /usr/local/lib )
ENDIF(APPLE)

##############
## Building ##
##############

INCLUDE(src/autogen_files.cmake)
add_python_export_library(${PROJECT_NAME} 
  ${PROJECT_SOURCE_DIR}/python/${PROJECT_NAME} ${AUTOGEN_FILES}
)

target_link_libraries(${PROJECT_NAME} 
                      ${catkin_LIBRARIES} ${Boost_LIBRARIES})
                      
INCLUDE(src/autogen_test_files.cmake)
add_python_export_library(${PROJECT_NAME}_test 
      ${PROJECT_SOURCE_DIR}/python/${PROJECT_NAME} ${AUTOGEN_TEST_FILES}
  )

#############
## Testing ##
#############
if(CATKIN_ENABLE_TESTING)

  ## Add nosetest based cpp test target.
  catkin_add_nosetests(test/numpy_eigen_tests.py)

endif()
