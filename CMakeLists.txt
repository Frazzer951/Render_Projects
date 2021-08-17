cmake_minimum_required(VERSION 3.11.0)
project(Render_Projects VERSION 1.0.0)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)

if (CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
  message("-- Compiling with MSVC")
  add_compile_options(-W4)
  string(REGEX REPLACE "/W3" "/W4" CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")
elseif (CMAKE_CXX_COMPILER_ID STREQUAL "Clang")
  message("-- Compiling with Clang")
  add_compile_options(-Wall -Wextra -Werror -Wdouble-promotion -fno-common -Wno-sign-compare)
else()
  message("-- Compiling with GCC")
  add_compile_options(-Wall -Wextra -Werror -Wdouble-promotion -Wconversion -fno-common -Wno-sign-compare)
endif()

list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)
include(ClangFormat)
include(FetchContent)

include_directories(include)

add_subdirectory(TinyRaytracer)

file(GLOB_RECURSE Sources
     "*.cpp"
     "*.hpp"
     "*.h"
)
list(FILTER Sources EXCLUDE REGEX "${PROJECT_SOURCE_DIR}/build/.*" )

clangformat_setup(
  ${Sources}
)