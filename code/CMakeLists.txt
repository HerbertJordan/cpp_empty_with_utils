#
# Builds the insieme core library
#
project(empty CXX)

include(cmake/environment.cmake)

# enable ctest support for testing
enable_testing()

# States that CMake required version must be >= 2.6
cmake_minimum_required(VERSION 2.6)

# Create a variable called insieme_compiler_core_SOURCES containing all .cpp files:
file(GLOB_RECURSE project_srcs 	src/*.cpp src/*.def src/*.inc )
file(GLOB_RECURSE project_incs 	include/*.h include/*.def include/*.inc )

# list all include paths
include_directories( ${project_inc_dir} )

# Build the code as a library
add_library(project_lib ${LINKING_TYPE} ${project_srcs} ${project_incs})

# Build executables

# Build unit tests
set ( ut_prefix  ut_ )
file(GLOB_RECURSE test_cases test/*.cc)
foreach ( case_file ${test_cases})
	get_filename_component( case_name ${case_file} NAME_WE )
	
	set ( case_name ${ut_prefix}${case_name} )
	add_executable(${case_name} ${case_file})

	target_link_libraries(${case_name} project_lib)
	target_link_libraries(${case_name} ${pthread_lib})

	add_unit_test(${case_name})
endforeach(case_file)

