# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

# Sets the minimum version of CMake required to build the native library.

cmake_minimum_required(VERSION 3.4.1)

# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds them for you.
# Gradle automatically packages shared libraries with your APK.
set(CURRENT_DIR ${CMAKE_SOURCE_DIR})

message("CURRENT_DIR:" ${CMAKE_SOURCE_DIR})

add_library( # Sets the name of the library.
             ffmpegjni

             # Sets the library as a shared library.
             SHARED

             # Provides a relative path to your source file(s).
             jni/ffmpeg-jni.cpp
             jni/myutils/utils.c
             jni/ffmpegprocess_plus/GetRgbFremer.cpp
             jni/ffmpegprocess_plus/H264Encoder.cpp
             jni/ffmpegprocess_plus/Mp4Muxer.cpp
             jni/ffmpegprocess_plus/RgbToYuv420P.cpp
             jni/ffmpegprocess_plus/cn_hongda_ffmpeg_library_Mp4Muxer.cpp
              )
add_library(ffmpeg-prebuilt SHARED  IMPORTED)
set_target_properties(ffmpeg-prebuilt PROPERTIES IMPORTED_LOCATION
    ${CMAKE_SOURCE_DIR}/jni/prebuilt/${ANDROID_ABI}/libffmpeg.so)

include_directories(${CMAKE_SOURCE_DIR}/jni/ffmpegprocess_plus/include)
#add share lib includ file
#target_include_directories(ffmpegjni PRIVATE ${CMAKE_SOURCE_DIR}/jni/ffmpegprocess_plus/include)
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
                       ffmpegjni
                       ffmpeg-prebuilt
                       # Links the target library to the log library
                       # included in the NDK.
                       ${log-lib} )