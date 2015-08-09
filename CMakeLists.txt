cmake_minimum_required(VERSION 3.3)

project(TexKit)

option(BUILD_SHARED_LIBS "Build shared library" ON)
option(BUILD_TESTING "Build unittests" OFF)
option(BUILD_EXAMPLES "Build examples" OFF)

set(CMAKE_CXX_COMPILER "/usr/bin/g++")
set(CMAKE_CXX_FLAGS "-std=c++14")
set(CMAKE_CXX_FLAGS_DEBUG "-g -Wall")
set(CMAKE_CXX_FLAGS_RELEASE "-O3")

set(TEXKIT_VERSION_MAJOR "0")
set(TEXKIT_VERSION_MINOR "1")
set(TEXKIT_VERSION_PATCH "0")
set(TEXKIT_VERSION_EXTRA "")
set(TEXKIT_VERSION "${TEXKIT_VERSION_MAJOR}.${TEXKIT_VERSION_MINOR}")
set(TEXKIT_VERSION_FULL "${TEXKIT_VERSION}.${TEXKIT_VERSION_PATCH}${TEXKIT_VERSION_EXTRA}")
set(LIB_SUFFIX "" CACHE STRING "Takes an empty string or 64. Directory where lib will be installed: lib or lib64")

set(includes
	${PROJECT_SOURCE_DIR}/include/TexKit/test.hpp
)

set(sources
	${PROJECT_SOURCE_DIR}/src/test.cpp
)

# Configure a header file to pass some of the CMake settings to the source code
configure_file(
	${PROJECT_SOURCE_DIR}/src/config.hpp.in
	${PROJECT_BINARY_DIR}/config.hpp
)

# Shared library unless BUILD_SHARED_LIBS is false
add_library(texkit ${sources} ${includes})

# Add include directories to target library
include_directories(${PROJECT_SOURCE_DIR}/include)

# If BUILD_TESTING, then build tests
if (BUILD_TESTING)
	enable_testing()
	add_executable(test_1 ${PROJECT_SOURCE_DIR}/tests/test_1.cpp ${includes})
	target_link_libraries(test_1 texkit)
	add_test(test_1 test_1)
endif (BUILD_TESTING)

# If BUILD_EXAMPLES, then build examples
if (BUILD_EXAMPLES)
	add_executable(example_1 ${PROJECT_SOURCE_DIR}/examples/example_1.cpp ${includes})
	target_link_libraries(example_1 texkit)
endif (BUILD_EXAMPLES)

install(FILES
	${PROJECT_BINARY_DIR}/config.hpp
	${PROJECT_SOURCE_DIR}/include/TexKit/test.hpp
	DESTINATION include/TexKit
)

install(TARGETS texkit DESTINATION lib)
