# Project details
cmake_minimum_required(VERSION 3.7)
project(DLFromScratch)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
option(BUILD_SHARED_LIBS "Build shared libraries" OFF)

# Custom libraries
file(GLOB_RECURSE LIB_SOURCES CONFIGURE_DEPENDS "libs/*.hpp" "libs/*.cpp" )
include_directories(${CMAKE_SOURCE_DIR})
include_directories (/usr/include/freetype2)

add_library(nn
    ${LIB_SOURCES}/nn/
)

add_library(nn
    ${LIB_SOURCES}/data/
)

add_library(nn
    ${LIB_SOURCES}/functional/
)

# External libraries
find_package(Armadillo REQUIRED)
include_directories(${ARMADILLO_INCLUDE_DIRS})

# Executable
add_executable(test src/test.cpp)

# Linkers
target_link_libraries(data ${ARMADILLO_LIBRARIES})
target_link_libraries(functional ${ARMADILLO_LIBRARIES})
target_link_libraries(nn ${ARMADILLO_LIBRARIES} functional data)
target_link_libraries(test nn ${ARMADILLO_LIBRARIES} functional data)

# Install
install(TARGETS test)
