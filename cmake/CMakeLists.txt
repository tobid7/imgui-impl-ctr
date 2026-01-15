cmake_minimum_required(VERSION 3.22)

project(imgui)

set(IMGUICTR_IMGUI_VERSION "v1.90.9" CACHE STRING "ImGui git tag / branch / commit")

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED true)


include(FetchContent)

FetchContent_Declare(
    imgui
    GIT_REPOSITORY https://github.com/ocornut/imgui.git
    GIT_TAG ${IMGUICTR_IMGUI_VERSION}
)

FetchContent_MakeAvailable(imgui)

add_library(imgui STATIC
    ${imgui_SOURCE_DIR}/imgui.cpp
    ${imgui_SOURCE_DIR}/imgui_demo.cpp
    ${imgui_SOURCE_DIR}/imgui_draw.cpp
    ${imgui_SOURCE_DIR}/imgui_tables.cpp
    ${imgui_SOURCE_DIR}/imgui_widgets.cpp
    ${imgui_SOURCE_DIR}/misc/cpp/imgui_stdlib.cpp
)

target_include_directories(imgui PUBLIC
    ${imgui_SOURCE_DIR}
    ${imgui_SOURCE_DIR}/misc/cpp
)

install(TARGETS imgui)
file(GLOB IMGUI_HEADERS "${imgui_SOURCE_DIR}/*.h")
install(FILES
    ${IMGUI_HEADERS}
    ${imgui_SOURCE_DIR}/misc/cpp/imgui_stdlib.h
    DESTINATION include
)
