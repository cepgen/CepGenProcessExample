cmake_minimum_required(VERSION 3.20 FATAL_ERROR)
project(CepGenProcessExample)  # replace this with the beautiful name of your process

set(CEPGEN_PATH ${CEPGEN_PATH} $ENV{CEPGEN_PATH})
set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake ${CEPGEN_PATH}/cmake)

find_package(CepGen REQUIRED)

file(GLOB SOURCES src/*.cpp)  # add as many sources as necessary
                              # (e.g. if you are interfacing another process implementation)

add_library(CepGenProcessExample SHARED ${SOURCES})
#target_link_libraries(CepGenSuperChic PRIVATE ${other_LIBRARIES})
target_include_directories(CepGenProcessExample PRIVATE ${CepGen_INCLUDE_DIRS})

file(GLOB_RECURSE input_cards RELATIVE ${PROJECT_SOURCE_DIR} cards/*)
foreach(_files ${input_cards})
  configure_file(${_files} ${_files} COPYONLY)
endforeach()
