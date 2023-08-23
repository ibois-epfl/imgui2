cmake_minimum_required(VERSION 3.1)
project(imgui)

add_library(imgui
    imgui.cpp
    imgui_draw.cpp
    imgui_demo.cpp
    imgui_widgets.cpp
    imgui_tables.cpp

    backends/imgui_impl_glfw.cpp
    backends/imgui_impl_opengl3.cpp
)
file(GLOB _headers *.h backends/*.h)

target_source(imgui PUBLIC
    FILE_SET HEADERS
      FILES ${_headers}
)

find_package(glfw3 REQUIRED)

if(NOT DEFINED(OpenGL_GL_PREFERENCE))
    set(OpenGL_GL_PREFERENCE LEGACY)
endif()
find_package(OpenGL REQUIRED)

target_link_libraries(imgui PRIVATE OpenGL::GL glfw)
target_include_directories(imgui
    INTERFACE $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}>
              $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/backends>
)

add_library(imgui::imgui ALIAS imgui)
