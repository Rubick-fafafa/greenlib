SET(EXECUTABLE "gsgpsocket_test_devices")

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -DUSE_GPSOCKET")

add_executable(${EXECUTABLE} example.cpp)
target_link_libraries(${EXECUTABLE} ${SystemC_LIBRARIES} pthread)

add_dependencies(compile_test ${EXECUTABLE})

add_test(NAME COMPILATION:_${EXECUTABLE}
         COMMAND  sh -c "./${EXECUTABLE} > executable_output.txt")

add_test(NAME TEMPLATE:_${EXECUTABLE}
         COMMAND ${CMAKE_COMMAND} -P ${CMAKE_CURRENT_SOURCE_DIR}/test.cmake)

add_test(NAME DIFF:_${EXECUTABLE}
         COMMAND ${CMAKE_COMMAND} -E compare_files executable_output.template
         ${CMAKE_CURRENT_SOURCE_DIR}/expected_output.template)