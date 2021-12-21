SET(CMAKE_EXPORT_COMPILE_COMMANDS ON)
project(pow_links)

cmake_minimum_required(VERSION 3.2)

# fetch submodules
execute_process (
    COMMAND bash -c "git submodule init && git submodule update && cd SHA3IUF && git checkout e909ac7791ac4335eb4382332ef56eb021dde482"
)

# compile options
set(CMAKE_CXX_STANDARD 17)

set(CMAKE_CXX_FLAGS "-Wall -static-libstdc++ -O3")
set(CMAKE_C_FLAGS "-Wall -static-libgcc -O3")

set(SOURCE_DIR "src/")

# generate in build/
set(LIB_PATH "${CMAKE_CURRENT_SOURCE_DIR}/build")
set(BIN_PATH "${CMAKE_CURRENT_SOURCE_DIR}/build")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${LIB_PATH})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${BIN_PATH})

# library
add_library(pow_links SHARED src/bridge.cpp src/bridge.def src/pow_links.cpp SHA3IUF/sha3.c)
add_executable(generator src/generator.cpp src/pow_links.cpp SHA3IUF/sha3.c)
