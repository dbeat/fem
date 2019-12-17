# Target:   poisson_test
# Type:     Executable
#========================================================================================
file(GLOB SOURCES *.cpp)

add_executable(${PROJECT_NAME}_test ${SOURCES})

target_link_libraries(${PROJECT_NAME}_test gtest Eigen3::Eigen ${DOLFIN_LIBRARIES})

# Register tests
add_test(NAME ${PROJECT_NAME}_test
         COMMAND ${PROJECT_NAME}_test)
