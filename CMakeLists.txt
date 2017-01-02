cmake_minimum_required (VERSION 3.6)
project (Optional)

# Uncomment the next three lines to compile with clang static analyzer
# set(CCC_CXX clang++)
# set(CMAKE_CXX_COMPILER /usr/lib/clang/c++-analyzer)
# set(CMAKE_LINKER clang++)

# Build System Information For Clang-Tidy
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
add_custom_target(compile_commands ALL
	COMMAND [ -f ${CMAKE_SOURCE_DIR}/build/compile_commands.json ] && mv ${CMAKE_SOURCE_DIR}/build/compile_commands.json
		${CMAKE_SOURCE_DIR}/compile_commands.json || :)

# Set Output Directories
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/bin)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/lib)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_SOURCE_DIR}/lib)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/lib")

# Set Compiler Flags
set(CMAKE_CXX_FLAGS "-std=c++14 -g -Wall")

# Include Directory
include_directories(include)

# Collect Test Sources
list(APPEND TEST_SOURCES "test/none_test.cpp"
						 "test/optional_test.cpp"
						 )

# Create Test Executable
add_executable(tests ${TEST_SOURCES})

# Link libraries to executables
target_link_libraries(tests gtest gtest_main)

#Install Headers
install(DIRECTORY include/ DESTINATION include/aml/optional)

# Doxygen Target - Doxyfile in project/docs - make docs
find_package(Doxygen)
if(DOXYGEN_FOUND)
add_custom_target(docs
${DOXYGEN_EXECUTABLE} ${CMAKE_SOURCE_DIR}/docs/Doxyfile
WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}/docs
COMMENT "Generating API documentation with Doxygen" VERBATIM)
endif(DOXYGEN_FOUND)