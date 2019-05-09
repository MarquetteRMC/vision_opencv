cmake_minimum_required(VERSION 2.8)
project(cv_bridge)

find_package(catkin REQUIRED COMPONENTS sensor_msgs roscpp  rosconsole  sensor_msgs)

FIND_PACKAGE(Boost)

if(NOT ANDROID)
  find_package(PythonLibs)
  if(PYTHONLIBS_VERSION_STRING VERSION_LESS 3)
    find_package(Boost REQUIRED python)
  else()
    find_package(Boost REQUIRED python3)
  endif()
else()
find_package(Boost REQUIRED)
endif()

if ( NOT (OpenCV_VERSION_MAJOR VERSION_LESS 4) )
find_package(OpenCV REQUIRED
  COMPONENTS
    opencv_core
    opencv_imgproc
    opencv_imgcodecs
    
  CONFIG
)
else()
find_package(OpenCV 3 REQUIRED
  COMPONENTS
    opencv_core
    opencv_imgproc
    opencv_highgui
  CONFIG
)
endif()


catkin_package(
  INCLUDE_DIRS include
  LIBRARIES ${PROJECT_NAME}
  CATKIN_DEPENDS rosconsole sensor_msgs
  DEPENDS OpenCV
  CFG_EXTRAS cv_bridge-extras.cmake
)

catkin_python_setup()

include_directories(include ${Boost_INCLUDE_DIRS} ${OpenCV_INCLUDE_DIRS} ${catkin_INCLUDE_DIRS})

#add_executable(image_converter_1 src/image_converter.cpp)
#target_link_libraries(image_converter_1 ${catkin_LIBRARIES} ${OpenCV_LIBS})

if(NOT ANDROID)
add_subdirectory(python)
endif()
add_subdirectory(src)
if(CATKIN_ENABLE_TESTING)
  add_subdirectory(test)
endif()

#add_executable(image_converter_1 src/image_converter.cpp)
#target_link_libraries(image_converter_1 ${OpenCV_LIBS})


# install the include folder
install(
  DIRECTORY include/${PROJECT_NAME}/
  DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION}
)

#add_executable(image_converter_1 src/image_converter.cpp)
#target_link_libraries(image_converter_1 ${OpenCV_LIBS})

