cmake_minimum_required(VERSION 3.5 FATAL_ERROR)

project(recipe-06 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Set the Eigen3 include directory if not defined
if(NOT DEFINED EIGEN3_INCLUDE_DIR)
  set(EIGEN3_INCLUDE_DIR "C:/Program Files/Eigen3")
endif()

# Add the Eigen3 include directory to the project
include_directories(${EIGEN3_INCLUDE_DIR})

# Attempt to find Eigen3
find_package(Eigen3 3.3 REQUIRED NO_MODULE PATHS ${EIGEN3_INCLUDE_DIR}/cmake)

if(Eigen3_FOUND)
  message(STATUS "Eigen3 found: ${EIGEN3_INCLUDE_DIR}")
  include_directories(${Eigen3_INCLUDE_DIR})
else()
  message(FATAL_ERROR "Eigen3 not found. Make sure Eigen3 is installed and the path is correct.")
endif()

include(CheckCXXCompilerFlag)

check_cxx_compiler_flag("-march=native" _march_native_works)
check_cxx_compiler_flag("-xHost" _xhost_works)

set(_CXX_FLAGS)
if(_march_native_works)
  message(STATUS "Using processor's vector instructions (-march=native compiler flag set)")
  set(_CXX_FLAGS "-march=native")
elseif(_xhost_works)
  message(STATUS "Using processor's vector instructions (-xHost compiler flag set)")
  set(_CXX_FLAGS "-xHost")
else()
  message(STATUS "No suitable compiler flag found for vectorization")
endif()

add_executable(linear-algebra-unoptimized linear-algebra.cpp)

target_link_libraries(linear-algebra-unoptimized
  PRIVATE
    Eigen3::Eigen
  )

add_executable(linear-algebra linear-algebra.cpp)

target_compile_options(linear-algebra
  PRIVATE
    ${_CXX_FLAGS}
  )

target_link_libraries(linear-algebra
  PRIVATE
    Eigen3::Eigen
  )
