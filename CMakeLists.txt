#
# This is a CMake makefile.  You can find the cmake utility and
# information about it at http://www.cmake.org
#


cmake_minimum_required(VERSION 2.8.4)

PROJECT(facial_control)

include(../dlib-18.17/dlib/cmake)

# Tell CMake to compile a program.  We do this with the ADD_EXECUTABLE()
# statement which takes the name of the output executable and then a list of
# .cpp files to compile.  Here each example consists of only one .cpp file but
# in general you will make programs that const of many .cpp files.
#ADD_EXECUTABLE(webcam_face_pose_ex webcam_face_pose_ex.cpp)
# Then we tell it to link with dlib.
#TARGET_LINK_LIBRARIES(webcam_face_pose_ex dlib)

#set (CMAKE_CXX_FLAGS "-O3 -pipe -march=armv7-a -mcpu=cortex-a9 -mfloat-abi=hard -DENABLE_NEON=ON")
#set (EXTRA_C_FLAGS_RELEASE "$(EXTRA_C_FLAGS_RELEASE) -02 -mfpu=neon")

# Since there are a lot of examples I'm going to use a macro to simply this
# CMakeLists.txt file.  However, usually you will create only one executable in
# your cmake projects and use the syntax shown above.
add_definitions(-msse4)

# For complilation on Odroid
# set(CMAKE_CXX_FLAGS "-mcpu=cortex-a9 -mfpu=neon mfloat-abi=softfp")

MACRO(add_example name)
   ADD_EXECUTABLE(${name} ${name}.cpp)
   TARGET_LINK_LIBRARIES(${name} dlib )
ENDMACRO()


#add_example(face_landmark_detection_ex)

find_package(OpenCV)
if (OpenCV_FOUND)
   include_directories(${OpenCV_INCLUDE_DIRS})

   ADD_EXECUTABLE(webcam_face_pose_ex webcam_face_pose_ex.cpp)
   TARGET_LINK_LIBRARIES(webcam_face_pose_ex dlib ${OpenCV_LIBS} )
endif()


if (DLIB_LINK_WITH_SQLITE3)
   add_example(sqlite_ex)
endif()

