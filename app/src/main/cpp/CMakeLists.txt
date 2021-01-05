cmake_minimum_required(VERSION 3.10.2)

enable_language(C CXX ASM)
set(CMAKE_ASM_COMPILER_TARGET "${CMAKE_CXX_COMPILER_TARGET}")

# 编译libjpegturbo
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/output/${ANDROID_ABI})
add_subdirectory(libjpegturbo)
add_library(libjpegturbo STATIC IMPORTED)
set_target_properties(libjpegturbo
        PROPERTIES
        IMPORTED_LOCATION ${PROJECT_SOURCE_DIR}/output/${ANDROID_ABI}/libturbojpeg.a)

find_library( # Defines the name of the path variable that stores the
        # location of the NDK library.
        jnigraphics-lib

        # Specifies the name of the NDK library that
        # CMake needs to locate.
        jnigraphics)

find_library( # Defines the name of the path variable that stores the
        # location of the NDK library.
        log-lib

        # Specifies the name of the NDK library that
        # CMake needs to locate.
        log)

find_library( # Defines the name of the path variable that stores the
        # location of the NDK library.
        dl-lib

        # Specifies the name of the NDK library that
        # CMake needs to locate.
        dl)


add_library( # Sets the name of the library.
        jni_jpegutil

        # Sets the library as a shared library.
        SHARED

        # Provides a relative path to your source file(s).
        jpegutil/jpegutil.cpp
        jpegutil/jpegutilnative.cpp)

target_link_libraries( # Specifies the target library.
        jni_jpegutil
        libjpegturbo

        # Links the target library to the log library
        # included in the NDK.
        ${dl-lib}
        ${log-lib}
        ${jnigraphics-lib})

add_library( # Sets the name of the library.
        jni_tinyplanet

        # Sets the library as a shared library.
        SHARED

        # Provides a relative path to your source file(s).
        tinyplanet/tinyplanet.cc)

target_link_libraries( # Specifies the target library.
        jni_tinyplanet

        # Links the target library to the log library
        # included in the NDK.
        ${log-lib}
        ${jnigraphics-lib})