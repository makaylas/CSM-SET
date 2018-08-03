cmake_minimum_required(VERSION 3.10)

# Link runSetTest with what we want to test and the GTest and pthread library
add_executable(runSetTests TestyMcTestFace.cpp)
target_link_libraries(runSetTests usgscsm ${GTEST_LIBRARIES} ${GTEST_MAIN_LIBRARIES} pthread)

gtest_discover_tests(runSetTests WORKING_DIRECTORY tests)
