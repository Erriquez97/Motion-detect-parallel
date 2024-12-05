cmake_minimum_required(VERSION 3.20)
project(VideoMotionDetect)

# Find OpenCV package
find_package(OpenCV REQUIRED)

# Include OpenCV directories
include_directories(${OpenCV_INCLUDE_DIRS})

# Include your custom include directory
include_directories("${CMAKE_SOURCE_DIR}/include")

# Create executable for VideoMotionDetect
add_executable(VideoMotionDetect VideoMotionDetect.cpp)

# Create executable for Statistics
add_executable(Statistics Statistics.cpp)

# Link OpenCV libraries to your executables
target_link_libraries(VideoMotionDetect ${OpenCV_LIBS})
target_link_libraries(Statistics ${OpenCV_LIBS})

# Set compiler flags (optional, but enables optimization)
SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -pthread -O3")
