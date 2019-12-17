# Target:   poisson
# Type:     Executable
#========================================================================================
file(GLOB SOURCES *.cpp)

add_executable(poisson ${SOURCES})
target_link_libraries (poisson Eigen3::Eigen ${DOLFIN_LIBRARIES})
