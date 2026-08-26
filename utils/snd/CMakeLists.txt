# Sound helper library, embedded from source (device build only;
# snd_sim.cpp is the Windows-simulator backend and is not compiled).
cmake_minimum_required(VERSION 3.15)

add_library(snd STATIC EXCLUDE_FROM_ALL
  src/snd_main.cpp
  src/snd_prizm.cpp)

target_include_directories(snd
  PUBLIC  "${CMAKE_CURRENT_SOURCE_DIR}/.."
  PRIVATE "${CMAKE_CURRENT_SOURCE_DIR}")

target_compile_options(snd PRIVATE
  $<$<COMPILE_LANGUAGE:C>:-std=c99>
  $<$<COMPILE_LANGUAGE:CXX>:-std=c++11 -fpermissive -fno-rtti -fno-exceptions
    -fno-threadsafe-statics -fno-use-cxa-get-exception-ptr>
  -Os -Wall -funroll-loops -fno-trapping-math -fno-trapv -Wno-switch)
