cmake_minimum_required (VERSION 3.0)
include(ExternalProject)
project (eigen-clang-weirdness)

set (CMAKE_INSTALL_PREFIX "${CMAKE_BINARY_DIR}/install"
	CACHE PATH "default install path" FORCE )
set(CMAKE_CXX_COMPILER clang-3.9)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++1y -lstdc++ -lm -O2")

ExternalProject_Add(
	eigen
	URL http://bitbucket.org/eigen/eigen/get/3.3.3.tar.gz
	URL_MD5 f21cee193e15e55cfd15ebbc16fc00a7
	CONFIGURE_COMMAND ""
	BUILD_COMMAND ""
	INSTALL_COMMAND
	${CMAKE_COMMAND} -E copy_directory
	${CMAKE_BINARY_DIR}/eigen-prefix/src/eigen/Eigen
	${CMAKE_INSTALL_PREFIX}/include/eigen/Eigen
	)
include_directories(${CMAKE_INSTALL_PREFIX}/include/eigen )

add_executable(failure_case failure_case.cc)
add_dependencies(failure_case eigen)

add_executable(success_case_extra_copy success_case_extra_copy.cc)
add_dependencies(success_case_extra_copy eigen)

install(TARGETS failure_case success_case_extra_copy 
	RUNTIME DESTINATION bin)
