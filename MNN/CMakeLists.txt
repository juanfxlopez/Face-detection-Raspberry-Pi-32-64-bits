# cmake needs this line
cmake_minimum_required(VERSION 3.1)

# add_compile_options(-D_UNICODE -DUNICODE)

# Enable C++14
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED TRUE)

set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_BUILD_TYPE Debug)

set(CMAKE_C_COMPILER /usr/bin/gcc)
set(CMAKE_CXX_COMPILER /usr/bin/g++)


# Define project name
set(PROJECT_NAME mnn_demo)
project(${PROJECT_NAME})

# set(CMAKE_SYSTEM_NAME Linux)
# set(CMAKE_SYSTEM_PROCESSOR arm)

# Find OpenCV, you may need to set OpenCV_DIR variable
# to the absolute path to the directory containing OpenCVConfig.cmake file
# via the command line or GUI
SET(OpenCV_DIR /usr/local/lib/cmake/opencv4)

find_package(OpenCV REQUIRED)

# add_library(mnn_demo SHARED
#                       MNN_UltraFace.hpp
#                       MNN_UltraFace.cpp
#                       main.cpp)

# find_library(MNN REQUIRED)

# find_library(MNN_Libs
#               libMNN.so
#               PATHS /usr/local/lib)

# If the package has been found, several variables will
# be set, you can find the full list with descriptions
# in the OpenCVConfig.cmake file.
# Print some message showing some of them
message(STATUS "OpenCV library status:")
message(STATUS "    config: ${OpenCV_DIR}")
message(STATUS "    version: ${OpenCV_VERSION}")
message(STATUS "    libraries: ${OpenCV_LIBS}")
message(STATUS "    include path: ${OpenCV_INCLUDE_DIRS}")

# Find or set the MNN library
find_library(MNN_LIBRARY_PATH NAMES libMNN.so)
#SET(MNN_LIBRARY_PATH "/usr/local/lib/libMNN.so")

# Add the MNN library to the project
add_library(MNN SHARED IMPORTED)
set_target_properties(MNN PROPERTIES IMPORTED_LOCATION ${MNN_LIBRARY_PATH})

add_executable(${PROJECT_NAME} main.cpp MNN_UltraFace.cpp MNN_UltraFace.hpp)

target_compile_options(${PROJECT_NAME} PRIVATE -Wall -fexceptions -pthread)

# Link your application with OpenCV and MNN libraries
target_include_directories(${PROJECT_NAME} PRIVATE /usr/local/include/MNN /usr/local/include/opencv4)
target_link_libraries(${PROJECT_NAME} LINK_PRIVATE -pthread ${OpenCV_LIBS} MNN)

#in /build ->after removing everything
#cmake -G 'Unix Makefiles' -DCMAKE_BUILD_TYPE=Debug ..
#make -j4
