cmake_minimum_required(VERSION 3.13.4)
enable_testing()
set(CMAKE_CXX_STANDARD 14)

set(MAIN_NAME dpcpp)
project(${MAIN_NAME})

file(GLOB_RECURSE SOURCES src/*.cpp)

include_directories(include)
add_executable(${MAIN_NAME} ${SOURCES})

# -------------------- gtest --------------------
configure_file(CMakeLists.txt.in googletest-download/CMakeLists.txt)
execute_process(COMMAND ${CMAKE_COMMAND} -G "${CMAKE_GENERATOR}" .
  RESULT_VARIABLE result
  WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/googletest-download )
if(result)
  message(FATAL_ERROR "CMake step for googletest failed: ${result}")
endif()
execute_process(COMMAND ${CMAKE_COMMAND} --build .
  RESULT_VARIABLE result
  WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/googletest-download )
if(result)
  message(FATAL_ERROR "Build step for googletest failed: ${result}")
endif()

set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)

add_subdirectory(${CMAKE_CURRENT_BINARY_DIR}/googletest-src
                 ${CMAKE_CURRENT_BINARY_DIR}/googletest-build
                 EXCLUDE_FROM_ALL)


function(dpcpp_test test_file)
  get_filename_component(test_target_name ${test_file} NAME_WE)

  add_executable(${test_target_name} ${test_file})
  target_link_libraries(${test_target_name} gtest_main)
  add_test(NAME ${test_target_name} COMMAND ${test_target_name})
endfunction()

dpcpp_test("test/dpcpp/creational/abstract_factory.cpp")
dpcpp_test("test/dpcpp/creational/builder.cpp")
dpcpp_test("test/dpcpp/creational/factory_method.cpp")
dpcpp_test("test/dpcpp/creational/prototype.cpp")
dpcpp_test("test/dpcpp/structural/adapter.cpp")
dpcpp_test("test/dpcpp/structural/bridge.cpp")
dpcpp_test("test/dpcpp/structural/composite.cpp")
dpcpp_test("test/dpcpp/structural/decorator.cpp")
dpcpp_test("test/dpcpp/structural/facade.cpp")
dpcpp_test("test/dpcpp/structural/flyweight.cpp")
dpcpp_test("test/dpcpp/structural/proxy.cpp")
dpcpp_test("test/dpcpp/behavioral/chain_of_responsibility.cpp")
dpcpp_test("test/dpcpp/behavioral/iterator.cpp")
dpcpp_test("test/dpcpp/behavioral/memento.cpp")
dpcpp_test("test/dpcpp/behavioral/state.cpp")
dpcpp_test("test/dpcpp/behavioral/template_method.cpp")
dpcpp_test("test/dpcpp/behavioral/command.cpp")
dpcpp_test("test/dpcpp/behavioral/mediator.cpp")
dpcpp_test("test/dpcpp/behavioral/observer.cpp")
dpcpp_test("test/dpcpp/behavioral/strategy.cpp")
dpcpp_test("test/dpcpp/behavioral/visitor.cpp")
