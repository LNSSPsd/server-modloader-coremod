cmake_minimum_required(VERSION 3.18)
project(CoreMod)

if (CMAKE_VERSION VERSION_LESS 3.0.0)
  set (CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")
else()
  set(CMAKE_CXX_STANDARD 11)
endif()
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_C_COMPILER clang)
set(CMAKE_CXX_COMPILER clang++)
set(CMAKE_POSITION_INDEPENDENT_CODE ON)

find_package(ModLoader REQUIRED)

add_library(CoreMod SHARED version_name.cpp log_forwarder.cpp common_hooks.cpp stub.c)
set_target_properties(CoreMod PROPERTIES COMPILE_FLAGS "-S")
add_custom_command(TARGET CoreMod PRE_LINK COMMAND node ${CMAKE_SOURCE_DIR}/preprocessor.js "\"$<TARGET_OBJECTS:CoreMod>\"")
target_link_libraries(CoreMod PUBLIC ModLoader)


