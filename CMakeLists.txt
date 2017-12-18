cmake_minimum_required(VERSION 3.0)

set(CMAKE_CXX_STANDARD 11)
include(cmake/HunterGate.cmake)

HunterGate(
URL "https://github.com/NeverMore27/hunter/archive/v0.19.137.12.tar.gz"
 SHA1 "9afc538a043fb9748a0d066684255a3b8cf9a30c"
)

project(demo)

hunter_add_package(avl_tree)
find_package(avl_tree)
add_executable(demo ${CMAKE_CURRENT_SOURCE_DIR}/sources/demo.cpp)
target_link_libraries(demo avl_tree)
install(TARGETS demo RUNTIME DESTINATION bin)