# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

# Sets the minimum version of CMake required to build the native library.
cmake_minimum_required(VERSION 3.5.1)

# Configure build library name.
set(TARGET_NAME native-lib)

# Build project shared lib
set(CMAKE_VERBOSE_MAKEFILE on)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -fexceptions -std=c++11")

set(LIBS_PATH /Users/miguel/idwall/android_libs)

# ------------------------------------------------------------------
#  OPENCV
# ------------------------------------------------------------------
add_library(opencv_lib SHARED IMPORTED)

# sets the location of the prebuilt opencv .so
set_target_properties( opencv_lib
                       PROPERTIES IMPORTED_LOCATION
                       ${LIBS_PATH}/opencv/lib/${ANDROID_ABI}/libopencv_java4.so )

include_directories(${LIBS_PATH}/opencv/include)

# ------------------------------------------------------------------
#  DLIB
# ------------------------------------------------------------------
add_library(dlib_lib SHARED IMPORTED)

# sets the location of the prebuilt dlib .so
set_target_properties( dlib_lib
                       PROPERTIES IMPORTED_LOCATION
                       ${LIBS_PATH}/dlib/lib/${ANDROID_ABI}/libdlib.so )

include_directories(${LIBS_PATH}/dlib/include)

# ------------------------------------------------------------------
#  Output Library
# ------------------------------------------------------------------
# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds them for you.
# Gradle automatically packages shared libraries with your APK.

add_library( # Sets the name of the library.
             ${TARGET_NAME}

             # Sets the library as a shared library.
             SHARED
             STATIC

             # Provides a relative path to your source file(s).
             src/main/cpp/native-lib.cpp )

target_include_directories(${TARGET_NAME} PRIVATE)

# Searches for a specified prebuilt library and stores the path as a
# variable. Because CMake includes system libraries in the search path by
# default, you only need to specify the name of the public NDK library
# you want to add. CMake verifies that the library exists before
# completing its build.

find_library( # Sets the name of the path variable.
              log-lib
              # Specifies the name of the NDK library that
              # you want CMake to locate.
              log )

# Specifies libraries CMake should link to your target library. You
# can link multiple libraries, such as libraries you define in this
# build script, prebuilt third-party libraries, or system libraries.

target_link_libraries( # Specifies the target library.
                       native-lib

                       opencv_lib
                       dlib_lib

                       # Links the target library to the log library
                       # included in the NDK.
                       ${log-lib} )
