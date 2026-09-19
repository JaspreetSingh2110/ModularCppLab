cmake_minimum_required(VERSION 4.3)
project(ModularCppLab)

set(CMAKE_CXX_STANDARD 26)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

option(DP_BUILD_LESSONS "Build lesson executables under lessons/" ON)
option(DP_BUILD_SAMPLE "Build the Phase D sample multi-lib project" OFF)
option(DP_BUILD_TESTS "Build tests" OFF)

# Scratch / hello executable
add_executable(playground app/playground/main.cpp)

if(DP_BUILD_LESSONS)
  add_subdirectory(lessons)
endif()

if(DP_BUILD_SAMPLE)
  add_subdirectory(sample)
endif()

if(DP_BUILD_TESTS)
  enable_testing()
  add_subdirectory(tests)
endif()
