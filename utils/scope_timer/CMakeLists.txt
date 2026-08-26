# ScopeTimer profiling helper, embedded from source.
# The whole implementation is guarded by #if DEBUG in scope_timer.cpp.
cmake_minimum_required(VERSION 3.15)

add_library(scope_timer STATIC EXCLUDE_FROM_ALL
  src/scope_timer.cpp)

target_include_directories(scope_timer
  PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/..")

target_compile_options(scope_timer PRIVATE
  $<$<COMPILE_LANGUAGE:CXX>:-std=c++11 -fpermissive -fno-rtti -fno-exceptions
    -fno-threadsafe-statics -fno-use-cxa-get-exception-ptr>
  -Os -Wall -funroll-loops -fno-trapping-math -fno-trapv -Wno-switch)
