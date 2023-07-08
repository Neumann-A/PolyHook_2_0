cmake_minimum_required(VERSION 3.25)

### Fetch Content approach from https://discourse.cmake.org/t/built-in-package-manager-for-cmake-modules/7513
include(FetchContent)
FetchContent_Populate(
  vcpkg
  GIT_REPOSITORY https://github.com/microsoft/vcpkg.git
  GIT_TAG        dafef74af53669ef1cc9015f55e0ce809ead62aa # latest as of 07/08/23
  SOURCE_DIR     "${CMAKE_SOURCE_DIR}/vcpkg"
  QUIET
)

list(APPEND CMAKE_PROJECT_TOP_LEVEL_INCLUDES "${vcpkg_SOURCE_DIR}/scripts/buildsystems/vcpkg.cmake")
list(APPEND CMAKE_TRY_COMPILE_PLATFORM_VARIABLES CMAKE_PROJECT_TOP_LEVEL_INCLUDES)
####

project(test_polyhook2)

find_package(PolyHook_2 CONFIG REQUIRED)

set(CMAKE_CXX_FLAGS "/std:c++latest ${CMAKE_CXX_FLAGS}")

add_executable(test_polyhook2 main.cpp)
set_property(TARGET test_polyhook2 PROPERTY MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")
target_link_libraries(test_polyhook2 PRIVATE PolyHook_2::PolyHook_2)
