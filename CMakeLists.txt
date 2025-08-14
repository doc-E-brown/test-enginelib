cmake_minimum_required(VERSION 3.10)
project(enginelib)

set(CMAKE_CXX_STANDARD 17)

add_library(enginelib SHARED enginelib.cpp)

# Specify include directories for consumers
target_include_directories(enginelib PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    $<INSTALL_INTERFACE:include>
)

set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)

# Install rules for CPM compatibility
install(TARGETS enginelib
    EXPORT enginelibTargets
    DESTINATION lib
)
install(FILES enginelib.h DESTINATION include)

# Export the targets for CPM
install(EXPORT enginelibTargets
    FILE enginelibTargets.cmake
    NAMESPACE enginelib::
    DESTINATION lib/cmake/enginelib
)

include(CMakePackageConfigHelpers)
write_basic_package_version_file(
    "${CMAKE_CURRENT_BINARY_DIR}/enginelibConfigVersion.cmake"
    VERSION "1.0.0"
    COMPATIBILITY AnyNewerVersion
)
configure_package_config_file(
    "${CMAKE_CURRENT_SOURCE_DIR}/cmake/enginelibConfig.cmake.in"
    "${CMAKE_CURRENT_BINARY_DIR}/enginelibConfig.cmake"
    INSTALL_DESTINATION "lib/cmake/enginelib"
)

install(FILES
    "${CMAKE_CURRENT_BINARY_DIR}/enginelibConfig.cmake"
    "${CMAKE_CURRENT_BINARY_DIR}/enginelibConfigVersion.cmake"
    DESTINATION lib/cmake/enginelib
)
