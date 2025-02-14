cmake_minimum_required(VERSION 3.5.0)
project(etl_error_handler_unit_tests)

add_definitions(-DETL_DEBUG)
add_definitions(-DETL_THROW_EXCEPTIONS)
add_definitions(-DETL_LOG_ERRORS)

include_directories(${PROJECT_SOURCE_DIR}/../../../include)

set(TEST_SOURCE_FILES
	test_error_handler.cpp
  )

add_executable(etl_tests
  ${TEST_SOURCE_FILES}
  )

if (ETL_OPTIMISATION MATCHES "-O1")
  message(STATUS "Compiling with -O1 optimisations")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O1")
endif()

if (ETL_OPTIMISATION MATCHES "-O2")
  message(STATUS "Compiling with -O2 optimisations")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O2")
endif()

if (ETL_OPTIMISATION MATCHES "-O3")
  message(STATUS "Compiling with -O3 optimisations")
  set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3")
endif()

target_include_directories(etl_tests
  PUBLIC
  ${CMAKE_CURRENT_LIST_DIR}
  )

if ((CMAKE_CXX_COMPILER_ID MATCHES "GNU") OR (CMAKE_CXX_COMPILER_ID MATCHES "Clang"))
	target_compile_options(etl_tests
			PRIVATE
			-fsanitize=address,undefined,bounds
			-fno-omit-frame-pointer
			-Wall
			-Wextra
			-Werror
			)
	target_link_options(etl_tests
			PRIVATE
			-fsanitize=address,undefined,bounds
			)
endif ()

# Enable the 'make test' CMake target using the executable defined above
add_test(etl_error_handler_unit_tests etl_tests)

# Since ctest will only show you the results of the single executable
# define a target that will output all of the failing or passing tests
# as they appear from UnitTest++
add_custom_target(test_verbose COMMAND ${CMAKE_CTEST_COMMAND} --verbose)


#RSG
set_property(TARGET etl_tests PROPERTY CXX_STANDARD 17)

