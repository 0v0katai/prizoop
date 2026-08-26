# ZX7 compression/decompression, embedded from source.
# Consumers include it as "zx7/zx7.h" relative to the utils/ root.
cmake_minimum_required(VERSION 3.15)

add_library(zx7 STATIC EXCLUDE_FROM_ALL
  src/compress.c
  src/dzx7.c
  src/optimize.c
  src/zx7.c)

target_include_directories(zx7
  PUBLIC  "${CMAKE_CURRENT_SOURCE_DIR}/.."
  PRIVATE "${CMAKE_CURRENT_SOURCE_DIR}")

target_compile_options(zx7 PRIVATE
  $<$<COMPILE_LANGUAGE:C>:-std=c99>
  -Os -Wall -funroll-loops -fno-trapping-math -fno-trapv -Wno-switch)
