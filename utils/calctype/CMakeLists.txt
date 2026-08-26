# Calctype text renderer, embedded from source.
# Fonts are #included directly by consumer code ("calctype/fonts/...").
cmake_minimum_required(VERSION 3.15)

add_library(calctype STATIC EXCLUDE_FROM_ALL
  src/calctype.c
  src/calctype_prizm.c)

target_include_directories(calctype
  PUBLIC  "${CMAKE_CURRENT_SOURCE_DIR}/.."
  PRIVATE "${CMAKE_CURRENT_SOURCE_DIR}")

target_compile_options(calctype PRIVATE
  $<$<COMPILE_LANGUAGE:C>:-std=c99>
  -Os -Wall -funroll-loops -fno-trapping-math -fno-trapv -Wno-switch)
