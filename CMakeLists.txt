cmake_minimum_required(VERSION 3.10)
project(csmset VERSION 0.1.1 DESCRIPTION "CSM-SET")

include(cmake/gtest.cmake)
include(GNUInstallDirs)
include(GoogleTest)

set(CMAKE_CXX_STANDARD 11)

set(COVERAGE OFF CACHE BOOL "Coverage")

add_subdirectory(sensorutils)
add_subdirectory(csmcameramodel)


add_library(csmset SHARED
            src/Set.cpp)

if(COVERAGE)
    target_compile_options(csmset PRIVATE --coverage -O0)
    target_link_libraries(csmset PRIVATE --coverage -O0)
endif()

set_target_properties(csmset PROPERTIES
    VERSION ${PROJECT_VERSION}
    SOVERSION 1
)


target_include_directories(csmset
                           PUBLIC
                           sensorutils/include/
                           sensorutils/include/sensorutils
                           csmcameramodel/include/
                           csmcameramodel/include/usgscam/
                           csmcameramodel/include/json/
)

find_package(Threads)

target_link_libraries(csmset PUBLIC
                      usgscsm
                      sensorutils
                      gtest ${CMAKE_THREAD_LIBS_INIT})

install(TARGETS csmset
    LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
    PUBLIC_HEADER DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})



include(CTest)

enable_testing()

add_subdirectory(tests)
