cmake_minimum_required (VERSION 2.8.7)

project(Stragnar_CV)

include(ExternalProject)
include(CheckCXXCompilerFlag)

# Set the version number
set (Stragnar_CV_VERSION_MAJOR 0)
set (Stragnar_CV_VERSION_MINOR 1)

CHECK_CXX_COMPILER_FLAG("-std=c++11" COMPILER_SUPPORTS_CXX11)
CHECK_CXX_COMPILER_FLAG("-std=c++0x" COMPILER_SUPPORTS_CXX0X)
if(COMPILER_SUPPORTS_CXX11)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -O3")
elseif(COMPILER_SUPPORTS_CXX0X)
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++0x -O3")
else()
    message(STATUS "The compiler ${CMAKE_CXX_COMPILER} has no C++11 support. Please use a different C++ compiler.")
endif()

# Configure a header to pass some CMake settings to the source.
configure_file (
    "${Stragnar_CV_SOURCE_DIR}/src/Stragnar_CV_Config.h.in"
    "${Stragnar_CV_BINARY_DIR}/src/Stragnar_CV_Config.h"
)

# Add the binary directory to the search include path to find 
# Stragnar_CV_Config.h
include_directories("${Stragnar_CV_BINARY_DIR}")
include_directories("${Stragnar_CV_SOURCE_DIR}")

# Add the 'hello world' executable
add_executable(hello_world src/hello_world)

# Get opencv
message( "External project - OpenCV" )

ExternalProject_Add(OpenCV
  GIT_REPOSITORY https://github.com/itseez/opencv.git 
  GIT_TAG 3.0.0-alpha
  SOURCE_DIR OpenCV 
  BINARY_DIR OpenCV-build
  INSTALL_DIR OpenCV-install 
  CMAKE_ARGS
    ${ep_common_args}
    -DBUILD_DOCS:BOOL=OFF
    -DBUILD_EXAMPLES:BOOL=OFF
    -DBUILD_NEW_PYTHON_SUPPORT:BOOL=OFF
    -DBUILD_PACKAGE:BOOL=ON
    -DBUILD_SHARED_LIBS:BOOL=ON
    -DBUILD_TESTS:BOOL=OFF
    -DWITH_FFMPEG:BOOL=OFF
    -DCMAKE_INSTALL_PREFIX:PATH=${CMAKE_BINARY_DIR}/INSTALL
)

find_package(OpenCV REQUIRED)
if (OpenCV_FOUND)
    add_executable(copy_matrix src/copy_matrix)
#    add_executable(video_cap src/video_cap)
    add_executable(webcam3d src/webcam3d)
    add_executable(warp_rotate src/warp_rotate)
    add_executable(reflection src/reflection)
    add_executable(rainbowfilter src/rainbowfilter)
    add_executable(psychedelic src/psychedelic)
    add_executable(plaid src/plaid)
    add_executable(pixelated src/pixelated)
    add_executable(pca src/pca)
    add_executable(myedgedetection src/myedgedetection)
    add_executable(kaleidoscope src/kaleidoscope)
    add_executable(edgedetection src/edgedetection)

    add_dependencies(copy_matrix OpenCV)
 #   add_dependencies(video_cap OpenCV)
    add_dependencies(webcam3d OpenCV)
    add_dependencies(warp_rotate OpenCV)
    add_dependencies(reflection OpenCV)
    add_dependencies(rainbowfilter OpenCV)
    add_dependencies(psychedelic OpenCV)
    add_dependencies(plaid OpenCV)
    add_dependencies(pixelated OpenCV)
    add_dependencies(pca OpenCV)
    add_dependencies(myedgedetection OpenCV)
    add_dependencies(kaleidoscope OpenCV)
    add_dependencies(edgedetection OpenCV)



    include_directories(${OpenCV_INCLUDE_DIRS})

    target_link_libraries(copy_matrix ${OpenCV_LIBRARIES})
  #  target_link_libraries(video_cap ${OpenCV_LIBRARIES})
    target_link_libraries(webcam3d ${OpenCV_LIBRARIES})
    target_link_libraries(warp_rotate ${OpenCV_LIBRARIES})
    target_link_libraries(reflection ${OpenCV_LIBRARIES})
    target_link_libraries(rainbowfilter ${OpenCV_LIBRARIES})
    target_link_libraries(psychedelic ${OpenCV_LIBRARIES})
    target_link_libraries(plaid ${OpenCV_LIBRARIES})
    target_link_libraries(pixelated ${OpenCV_LIBRARIES})
    target_link_libraries(pca ${OpenCV_LIBRARIES})
    target_link_libraries(myedgedetection ${OpenCV_LIBRARIES})
    target_link_libraries(kaleidoscope ${OpenCV_LIBRARIES})
    target_link_libraries(edgedetection ${OpenCV_LIBRARIES})

endif (OpenCV_FOUND)

