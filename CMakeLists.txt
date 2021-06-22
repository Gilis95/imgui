cmake_minimum_required(VERSION 3.16)
project(imgui-dear VERSION 1.1.3 LANGUAGES CXX)
set (CMAKE_CXX_STANDARD 17)

include(GNUInstallDirs)

set(HDR_DIR ${PROJECT_SOURCE_DIR}/)
set(SRC_DIR ${PROJECT_SOURCE_DIR}/)

set(BACKEND_HDR_DIR ${HDR_DIR}/backends)
set(BACKEND_SRC_DIR ${SRC_DIR}/backends)

set(SRC_INSTALL_DIR ${CMAKE_INSTALL_FULL_INCLUDEDIR}/imgui/src)
set(SRC_BACKEND_INSTALL_DIR ${SRC_INSTALL_DIR}/backend)

file(GLOB ROOT_SOURCE ${SRC_DIR}/*.cpp)
file(GLOB ROOT_HEADER ${HDR_DIR}/*.h)
file(GLOB ROOT_INLINE ${HDR_DIR}/*.hpp)

file(GLOB BACKEND_SOURCE ${BACKEND_SRC_DIR}/*.cpp)
file(GLOB BACKEND_HEADER ${BACKEND_HDR_DIR}/*.h)
file(GLOB BACKEND_INLINE ${BACKEND_HDR_DIR}/*.hpp)

add_library(imgui INTERFACE)

target_include_directories(imgui INTERFACE
        $<BUILD_INTERFACE:${HDR_DIR}>
        $<INSTALL_INTERFACE:include>)

target_sources(imgui INTERFACE
        $<BUILD_INTERFACE:${SRC_DIR}>
        $<INSTALL_INTERFACE:${SRC_INSTALL_DIR}>)

option(IMGUI_INSTALL "Generate installation target" OFF)

if(IMGUI_INSTALL)
    set(version_config "${CMAKE_CURRENT_BINARY_DIR}/imguiConfigVersion.cmake")
    set(project_config "${CMAKE_CURRENT_BINARY_DIR}/imguiConfig.cmake")
    set(targets_export_name "imguiTargets")
    set(namespace "imgui::")

    # Include module with fuction 'write_basic_package_version_file'
    include(CMakePackageConfigHelpers)

    # Configure 'imguiConfigVersion.cmake'
    # PROJECT_VERSION is used as a VERSION
    write_basic_package_version_file("${version_config}" COMPATIBILITY SameMajorVersion)

    # Configure 'imguiConfig.cmake'
    # Uses targets_export_name variable.
    configure_package_config_file(
            "Config.cmake.in"
            "${project_config}"
            INSTALL_DESTINATION ${CMAKE_INSTALL_FULL_LIBDIR}/cmake/imgui)

    # Targets:
    install(
            TARGETS imgui
            EXPORT "${targets_export_name}"
            LIBRARY DESTINATION ${CMAKE_INSTALL_FULL_LIBDIR}
            ARCHIVE DESTINATION ${CMAKE_INSTALL_FULL_LIBDIR}
            RUNTIME DESTINATION ${CMAKE_INSTALL_FULL_BINDIR})

    install(FILES ${ROOT_HEADER} ${ROOT_INLINE} ${ROOT_SOURCE}
            DESTINATION ${SRC_INSTALL_DIR})

    install(FILES ${BACKEND_HEADER} ${BACKEND_INLINE} ${BACKEND_SOURCE}
            DESTINATION ${SRC_BACKEND_INSTALL_DIR})

    # Install imguiConfig.cmake, imguiConfigVersion.cmake
    install(
            FILES "${project_config}" "${version_config}"
            DESTINATION ${CMAKE_INSTALL_FULL_LIBDIR}/cmake/imgui)

    # Create and install imguiTargets.cmake
    install(
            EXPORT "${targets_export_name}"
            NAMESPACE "${namespace}"
            DESTINATION ${CMAKE_INSTALL_FULL_LIBDIR}/cmake/imgui)
endif()