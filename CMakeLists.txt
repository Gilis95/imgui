cmake_minimum_required(VERSION 3.16)
project(imgui-dear)

set (CMAKE_CXX_STANDARD 17)

set(HDR_DIR ${PROJECT_SOURCE_DIR}/)
set(SRC_DIR ${PROJECT_SOURCE_DIR}/)

set(HDR_FILES
        ${HDR_DIR}/imconfig.h
        ${HDR_DIR}/imgui.h
        ${HDR_DIR}/imgui_internal.h
        ${HDR_DIR}/imstb_rectpack.h
        ${HDR_DIR}/imstb_textedit.h
        ${HDR_DIR}/imstb_truetype.h
        ${HDR_DIR}/backends/imgui_impl_glfw.h
        ${HDR_DIR}/backends/imgui_impl_opengl3.h
        )

set(SRC_FILES
        ${SRC_DIR}/imgui.cpp
        ${SRC_DIR}/imgui_demo.cpp
        ${SRC_DIR}/imgui_draw.cpp
        ${SRC_DIR}/imgui_widgets.cpp
        ${SRC_DIR}/backends/imgui_impl_glfw.cpp
        ${HDR_DIR}/backends/imgui_impl_opengl3.cpp
        )

add_library(imgui-dear STATIC ${SRC_FILES} ${HDR_FILES})

target_include_directories(imgui-dear PRIVATE
        ${HDR_DIR}
        ${OPENGL_INCLUDE_DIRS}
        ${WINDOW_ABSTRACTION_INCLUDE_DIR}
        )

target_link_libraries(imgui-dear PRIVATE
        ${OPENGL_LIB}
        ${WINDOW_ABSTRACTION_LIB}
        )
