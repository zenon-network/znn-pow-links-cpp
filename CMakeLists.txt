cmake_minimum_required(VERSION 3.14)

project(pow_links)

enable_language(CXX) 

execute_process (
    COMMAND bash -c "git submodule init && git submodule update && cd SHA3IUF && git checkout e909ac7791ac4335eb4382332ef56eb021dde482"
)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(CMAKE_CXX_STANDARD 17)

if(${CMAKE_SYSTEM_NAME} MATCHES "Windows")
set(CMAKE_CXX_FLAGS "-Wall -static -static-libstdc++ -O3")
set(CMAKE_C_FLAGS "-Wall -static -static-libgcc -O3")
else()
set(CMAKE_CXX_FLAGS "-Wall -static-libstdc++ -O3")
set(CMAKE_C_FLAGS "-Wall -static-libgcc -O3")
endif()


set(SOURCE_DIR "src/")

set(LIB_PATH "${CMAKE_CURRENT_SOURCE_DIR}/build")
set(BIN_PATH "${CMAKE_CURRENT_SOURCE_DIR}/build")
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${LIB_PATH})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${BIN_PATH})

set(public_headers "${CMAKE_CURRENT_SOURCE_DIR}/src/bridge.hpp")

set(CMAKE_XCODE_ATTRIBUTE_EXCLUDED_ARCHS[sdk=iphonesimulator*] "")
set(CMAKE_OSX_ARCHITECTURES "arm64;x86_64" CACHE STRING "" FORCE)

add_library(pow_links SHARED src/bridge.hpp src/bridge.cpp src/bridge.def src/pow_links.cpp SHA3IUF/sha3.c)

if(CMAKE_SYSTEM MATCHES Windows OR CMAKE_SYSTEM MATCHES Linux OR CMAKE_SYSTEM MATCHES Darwin)
    add_executable(generator src/generator.cpp src/pow_links.cpp SHA3IUF/sha3.c)
endif()

set_target_properties(pow_links PROPERTIES
    FRAMEWORK TRUE
    FRAMEWORK_VERSION C
    MACOSX_FRAMEWORK_IDENTIFIER network.zenon.powlinks
    VERSION 0.0.1
    SOVERSION 1.0.0
    PUBLIC_HEADER "${public_headers}"
)