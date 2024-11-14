cmake_minimum_required(VERSION 3.10)
project(ProgramName)

set(CMAKE_CXX_STANDARD 17) # Or whichever version you're using
include_directories(include)

# Add source files
file(GLOB SOURCES "src/*.cpp")

# Define the executable
add_executable(Program ${SOURCES})
