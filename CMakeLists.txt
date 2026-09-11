cmake_minimum_required(VERSION 3.15)
project(prizoop LANGUAGES C CXX ASM VERSION "01.11.0000")

include(GenerateG3A)
include(FetchContent)

configure_file(src/config.h.in include/config.h)
add_compile_definitions(TARGET_PRIZM=1 DEBUG=0)

file(GLOB_RECURSE SOURCES CONFIGURE_DEPENDS
  "${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp"
  "${CMAKE_CURRENT_SOURCE_DIR}/src/*.c"
  "${CMAKE_CURRENT_SOURCE_DIR}/src/*.S")

include_directories(
  "${CMAKE_CURRENT_SOURCE_DIR}/src"
  "${CMAKE_CURRENT_SOURCE_DIR}/include"
  "${CMAKE_CURRENT_BINARY_DIR}/include")

FetchContent_Declare(zx7
  GIT_REPOSITORY https://github.com/0v0katai/prizm-zx7
  GIT_TAG master
  SOURCE_DIR "${CMAKE_CURRENT_BINARY_DIR}/_deps/zx7")
FetchContent_Declare(calctype
  GIT_REPOSITORY https://github.com/0v0katai/calctype
  GIT_TAG master
  SOURCE_DIR "${CMAKE_CURRENT_BINARY_DIR}/_deps/calctype")
FetchContent_Declare(snd
  GIT_REPOSITORY https://github.com/0v0katai/prizm-snd
  GIT_TAG master
  SOURCE_DIR "${CMAKE_CURRENT_BINARY_DIR}/_deps/snd")
FetchContent_Declare(scope_timer
  GIT_REPOSITORY https://github.com/0v0katai/scope_timer
  GIT_TAG main
  SOURCE_DIR "${CMAKE_CURRENT_BINARY_DIR}/_deps/scope_timer")
FetchContent_Declare(ptune2_simple
  GIT_REPOSITORY https://github.com/0v0katai/ptune2_simple
  GIT_TAG master
  SOURCE_DIR "${CMAKE_CURRENT_BINARY_DIR}/_deps/ptune2_simple")
FetchContent_MakeAvailable(zx7 calctype snd scope_timer ptune2_simple)

add_executable(prizoop ${SOURCES})

target_compile_options(prizoop PRIVATE
  -O2 -Wall -funroll-loops -fno-trapping-math -fno-trapv -Wno-switch
  $<$<COMPILE_LANGUAGE:C>:-std=c99>
  $<$<COMPILE_LANGUAGE:CXX>:-std=c++11 -fpermissive -fno-rtti
    -fno-exceptions -fno-threadsafe-statics
    -fno-use-cxa-get-exception-ptr>)

target_link_options(prizoop PRIVATE
  -L "${CMAKE_CURRENT_SOURCE_DIR}"
  -T "${CMAKE_CURRENT_SOURCE_DIR}/prizm.ld"
  -Wl,-Map=map -Wl,-print-memory-usage
  -Wl,-gc-sections)

target_link_libraries(prizoop PRIVATE
  zx7 calctype snd scope_timer ptune2_simple
  -lsyscall-${PLATFORM} -lc -lgcc)

if("${FXSDK_PLATFORM_LONG}" STREQUAL fxCG50)
  if("${PLATFORM}" STREQUAL cw)
    generate_g3a(TARGET prizoop OUTPUT "prizoop_cw.g3a"
      NAME "Prizoop" ICONS icon-uns.png icon-sel.png)
  else()
    generate_g3a(TARGET prizoop OUTPUT "prizoop_cg.g3a"
      NAME "Prizoop" ICONS icon-uns.png icon-sel.png)
  endif()
endif()