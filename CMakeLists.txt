cmake_minimum_required(VERSION 3.8)

set(CMAKE_CXX_STANDARD 11)
include(cmake/HunterGate.cmake)

HunterGate(
URL "https://github.com/NeverMore27/hunter/archive/v0.19.137.12.tar.gz"
 SHA1 "9afc538a043fb9748a0d066684255a3b8cf9a30c"
)

project(demo)
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

option(BUILD_TESTS "Build tests" OFF)
option(BUILD_EXAMPLES "Build Examples" OFF)

add_library(${PROJECT_NAME} INTERFACE)
target_sources(${PROJECT_NAME} INTERFACE "${PROJECT_SOURCE_DIR}/include/test.hpp")

target_include_directories(${PROJECT_NAME} INTERFACE "${PROJECT_SOURCE_DIR}/include")

if(BUILD_TESTS)
  hunter_add_package(Catch)
  hunter_add_package(avl_tree)
  find_package(Catch CONFIG REQUIRED)
  
  enable_testing()

  file(GLOB ${PROJECT_NAME}_TEST_SOURCES tests/*.cpp)
  add_executable(tests ${${PROJECT_NAME}_TEST_SOURCES})
  target_link_libraries(tests ${PROJECT_NAME} Catch::Catch )

  add_test(NAME test_name COMMAND tests "-s" "-r" "compact" "--use-colour" "yes") 
endif()

if(BUILD_EXAMPLES)
    file(GLOB EXAMPLE_SOURCES "${CMAKE_CURRENT_SOURCE_DIR}/examples/*.cpp")
    foreach(EXAMPLE_SOURCE ${EXAMPLE_SOURCES})
        get_filename_component(EXAMPLE_NAME ${EXAMPLE_SOURCE} NAME_WE)
        set(EXAMPLE_TARGET_NAME example_${EXAMPLE_NAME})
        add_executable(${EXAMPLE_TARGET_NAME} ${EXAMPLE_SOURCE})
        target_link_libraries(${EXAMPLE_TARGET_NAME} ${PROJECT_NAME})
        set_target_properties(${EXAMPLE_TARGET_NAME} PROPERTIES OUTPUT_NAME ${EXAMPLE_NAME})
        install(TARGETS ${EXAMPLE_TARGET_NAME}
                RUNTIME DESTINATION bin
                )
    endforeach(EXAMPLE_SOURCE ${EXAMPLE_SOURCES})
endif()
