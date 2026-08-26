# Ptune2 (simple) overclocking helper, embedded from source.
cmake_minimum_required(VERSION 3.15)

add_library(ptune2_simple STATIC EXCLUDE_FROM_ALL
  src/Ptune2_direct.c)

target_include_directories(ptune2_simple
  PUBLIC  "${CMAKE_CURRENT_SOURCE_DIR}/.."
  PRIVATE "${CMAKE_CURRENT_SOURCE_DIR}")

target_compile_options(ptune2_simple PRIVATE
  $<$<COMPILE_LANGUAGE:C>:-std=c99>
  -Os -Wall -funroll-loops -fno-trapping-math -fno-trapv -Wno-switch)
