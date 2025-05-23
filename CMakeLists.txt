cmake_minimum_required(VERSION 3.20 FATAL_ERROR)
if(NOT PROCESS_NAME)
  set(PROCESS_NAME CepGenProcessExample)  # replace this with the beautiful name of your process
endif()

project(${PROCESS_NAME})

set(CEPGEN_PATH ${CEPGEN_PATH} $ENV{CEPGEN_PATH})
set(CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake ${CEPGEN_PATH}/share/cmake ${CEPGEN_PATH}/cmake)

find_package(CepGen REQUIRED)

file(GLOB SOURCES src/*.cpp)  # add as many sources as necessary
                              # (e.g. if you are interfacing another process implementation)

add_library(${PROCESS_NAME} SHARED ${SOURCES})
target_link_libraries(${PROCESS_NAME} PRIVATE CepGen::CepGen)

# in case you need to link your process definition with other external libraries, you can use
# the standard CMake directives:
#target_link_libraries(${PROCESS_NAME} PUBLIC ${other_LIBRARIES})
#target_include_directories(${PROCESS_NAME} PRIVATE ${other_INCLUDE_DIRS})

file(GLOB_RECURSE input_cards RELATIVE ${PROJECT_SOURCE_DIR} cards/*)
foreach(_files ${input_cards})
  configure_file(${_files} ${_files} COPYONLY)
endforeach()
