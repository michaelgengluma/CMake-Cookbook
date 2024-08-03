# Set minimum CMake version
cmake_minimum_required(VERSION 3.5 FATAL_ERROR)

# Project name and language
project(recipe-08 LANGUAGES CXX)

# Print C++ compiler flags
message("C++ compiler flags: ${CMAKE_CXX_FLAGS}")

# List of flags
list(APPEND flags "-Wall")
if(NOT WIN32)
  list(APPEND flags "-fPIC" "-Wextra" "-Wpedantic")
else()
  # Add MSVC specific flags
  if(MSVC)
    list(APPEND flags "/W4")
  endif()
endif()

# Define the library
add_library(geometry
  STATIC
    geometry_circle.cpp
    geometry_circle.hpp
    geometry_polygon.cpp
    geometry_polygon.hpp
    geometry_rhombus.cpp
    geometry_rhombus.hpp
    geometry_square.cpp
    geometry_square.hpp
  )

# Apply compile options to the library
target_compile_options(geometry
  PRIVATE
    ${flags}
  )

# Define the executable
add_executable(compute-areas compute-areas.cpp)

# Apply compile options to the executable
if(NOT WIN32)
  target_compile_options(compute-areas
    PRIVATE
      "-fPIC"
  )
else()
  if(MSVC)
    target_compile_options(compute-areas
      PRIVATE
        "/W4"
    )
  endif()
endif()

# Link the library to the executable
target_link_libraries(compute-areas geometry)
