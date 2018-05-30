cmake_minimum_required(VERSION 3.10)
project(FakeIt CXX)
set(CMAKE_CXX_STANDARD 17)

add_library(FakeIt INTERFACE)
target_include_directories(FakeIt INTERFACE
       # $<BUILD_INTERFACE:"${CMAKE_CURRENT_SOURCE_DIR}/include">
#        $<INSTALL_INTERFACE:include>
        #$<INSTALL_INTERFACE:config/standalone>
        )
set(include_install_dir "include")
set(lib_install_dir "lib/")
set(bin_install_dir "bin/")
set(config_install_dir "lib/cmake/${PROJECT_NAME}")

set(generated_dir "${CMAKE_CURRENT_BINARY_DIR}/generated")

set(project_config "${generated_dir}/${PROJECT_NAME}Config.cmake")
set(targets_export_name "${PROJECT_NAME}Targets")

set(namespace "${PROJECT_NAME}::")

configure_file("${PROJECT_SOURCE_DIR}/cmake/Config.cmake.in" "${project_config}" @ONLY)

# Install target (will install the library to specified CMAKE_INSTALL_PREFIX variable)
install(
        TARGETS FakeIt
        EXPORT ${targets_export_name}
        ARCHIVE DESTINATION ${lib_install_dir}
        LIBRARY DESTINATION ${lib_install_dir}
        RUNTIME DESTINATION ${bin_install_dir}
        INCLUDES DESTINATION ${include_install_dir})

install(
        DIRECTORY "${PROJECT_SOURCE_DIR}/include/"
        DESTINATION ${include_install_dir}
        FILES_MATCHING PATTERN "*.*hpp")
install(
        DIRECTORY "${PROJECT_SOURCE_DIR}/tests/"
        DESTINATION ${include_install_dir}
        FILES_MATCHING PATTERN "*.*hpp")
install(
        DIRECTORY "${PROJECT_SOURCE_DIR}/config/standalone/"
        DESTINATION ${include_install_dir}
        FILES_MATCHING PATTERN "*.*hpp")

install(
        FILES "${project_config}"
        DESTINATION "${config_install_dir}")

install(
        EXPORT "${targets_export_name}"
        NAMESPACE "${namespace}"
        DESTINATION "${config_install_dir}")