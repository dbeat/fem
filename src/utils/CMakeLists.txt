# Target:   utils
# Type:     Executable
#========================================================================================
file(GLOB SOURCES *.cpp)

add_executable(utils ${SOURCES})
target_link_libraries (utils Eigen3::Eigen ${DOLFIN_LIBRARIES})