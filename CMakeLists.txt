cmake_minimum_required (VERSION 3.5)
project (poisson)
set(PROJECT_VERSION_MAJOR "0")
set(PROJECT_VERSION_MINOR "1")
set(PROJECT_VERSION_PATCH "0")
set(PROJECT_DESCRIPTION  "A code to demonstrate the discrete Galerkin method.")
set(PROJECT_AUTHOR  "Francois Roy")
set(PROJECT_URL  "https://github.com/frRoy/DG")

# Compile the Poisson.ufl form with FFC: ffc -l dolfin Poisson.ufl
# in order to generate Poisson.h

# Require and use C++11
# Use C++11
set(CMAKE_CXX_STANDARD 11)
# Require C++11
set(CMAKE_CXX_STANDARD_REQUIRED ON)
# Do not enable compiler-specific extensions
# set(CMAKE_CXX_EXTENSIONS OFF)

set(CMAKE_BUILD_TYPE Release)
# Debugging Options
set (CMAKE_VERBOSE_MAKEFILE 0) # Use 1 for debugging, 0 otherwise.

set(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)
set(CMAKE_CONFIG_PATH ${PROJECT_SOURCE_DIR}/cmake.config)

find_package (DOLFIN REQUIRED)
find_package (Eigen3 3.3 REQUIRED NO_MODULE)
find_package( Boost REQUIRED COMPONENTS timer )
find_package (PETSC REQUIRED)
find_package (SLEPC REQUIRED)
find_package(Git REQUIRED)

# include_directories( ${Boost_INCLUDE_DIRS} )

# Eigen exports a CMake target called Eigen3::Eigen which can be imported using the find_package CMake command and used by calling target_link_libraries
if (TARGET Eigen3::Eigen)
  # message("EIGEN FOUND")
endif (TARGET Eigen3::Eigen)

if (DOLFIN_FOUND)
  # message("DOLFIN FOUND")
  include_directories(${DOLFIN_INCLUDE_DIRS})
  # include_directories(SYSTEM ${DOLFIN_3RD_PARTY_INCLUDE_DIRS})
endif (DOLFIN_FOUND)

include_directories(${PROJECT_BINARY_DIR} ${PROJECT_SOURCE_DIR}/include)
include(DownloadProject)

# Compiler definitions
# add_definitions(${DOLFIN_CXX_DEFINITIONS})
# Compiler flags
# set(CMAKE_CXX_FLAGS "${DOLFIN_CXX_FLAGS} ${CMAKE_CXX_FLAGS}")

message("Downloading GoogleTest")
# Build Google test and google mock no matter what
download_project(PROJ                googletest
                 GIT_REPOSITORY      https://github.com/google/googletest.git
                 GIT_TAG             master
                 UPDATE_DISCONNECTED 1
                 QUIET
)
message("-- Building GoogleTest")
add_subdirectory(${googletest_SOURCE_DIR} ${googletest_BINARY_DIR} EXCLUDE_FROM_ALL)
include_directories("${gtest_SOURCE_DIR}/include"
                    "${gmock_SOURCE_DIR}/include")

# compile code
add_subdirectory(src)
add_subdirectory(src/utils)

# documentation
set(INPUT_DOC  
  ${PROJECT_SOURCE_DIR}/doc
  ${PROJECT_SOURCE_DIR}/src
  ${PROJECT_SOURCE_DIR}/src/utils
  ${PROJECT_SOURCE_DIR}/include)
string (REPLACE ";" " " INPUT_DOC "${INPUT_DOC}")
add_subdirectory(doc)

# tests
include(CTest)
add_test (poissonRuns ${CMAKE_BINARY_DIR}/src/poisson)
add_subdirectory(${PROJECT_SOURCE_DIR}/tests)
