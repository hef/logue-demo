cmake_minimum_required(VERSION 3.22)

include(FetchContent)

add_subdirectory(toolchain)

FetchContent_Declare(
        logue-sdk
        GIT_REPOSITORY https://github.com/korginc/logue-sdk.git
        GIT_TAG  ab654af0e29e5eb6eab7289d05699b45417d1f68
)
FetchContent_MakeAvailable(logue-sdk)

project(logue-demo)

add_subdirectory(minilouge-xd)

add_executable(waves waves.cpp waves.hpp)
set_target_properties(waves
        PROPERTIES
        SUFFIX ".elf"
        )

target_link_libraries(waves minilouge-xd-platform)

add_custom_command(
        OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/payload.bin
        MAIN_DEPENDENCY waves
        COMMAND "${CMAKE_OBJCOPY}"
        ARGS -O binary $<TARGET_FILE:waves> ${CMAKE_CURRENT_BINARY_DIR}/payload.bin
        )

add_custom_command(
        OUTPUT ${CMAKE_CURRENT_BINARY_DIR}/waves.mnlgxdunit
        MAIN_DEPENDENCY ${CMAKE_CURRENT_BINARY_DIR}/payload.bin
        COMMAND ${CMAKE_COMMAND}
        ARGS -E tar cv ${CMAKE_CURRENT_BINARY_DIR}/waves.mnlgxdunit --format=zip --
        ${CMAKE_CURRENT_BINARY_DIR}/payload.bin
        ${CMAKE_CURRENT_SOURCE_DIR}/manifest.json
)

add_custom_target(package DEPENDS ${CMAKE_CURRENT_BINARY_DIR}/waves.mnlgxdunit)