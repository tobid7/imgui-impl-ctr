cmake_minimum_required(VERSION 3.22)

find_program(PICASSO NAMES picasso REQUIRED)

# Function origanally Created in Re-Craft-3DS
function(__imgui_ctr_make_shader arg1 arg2)
    # These only exist cause i was stupid
    set(__FILE ${arg1})
    set(__NAME ${arg2})

    # Need to build shaders during config stage :(
    execute_process(
        COMMAND ${PICASSO} -o "${CMAKE_BINARY_DIR}/${__NAME}.shbin" "${__FILE}"
        RESULT_VARIABLE WORLD_RESULT
        WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
    )
    dkp_add_embedded_binary_library(${__NAME}_asm "${CMAKE_BINARY_DIR}/${__NAME}.shbin")
endfunction()

project(imgui-ctr VERSION 1.89.9)

option(IMGUI_CTR_BUILD_EXAMPLE "Build with example" OFF)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED true)

add_subdirectory(vendor)

__imgui_ctr_make_shader("${CMAKE_CURRENT_SOURCE_DIR}/source/imgui_impl_c3d.v.pica" imgui_impl_c3d)

add_library(imgui-ctr STATIC
    source/imgui_impl_ctr.cpp
    source/imgui_impl_citro3d.cpp
)
target_include_directories(imgui-ctr PUBLIC
    include
)
target_link_libraries(imgui-ctr PUBLIC
    ctru
    citro3d
    imgui
    imgui_impl_c3d_asm
)

add_dependencies(imgui-ctr imgui_impl_c3d_asm)

install(DIRECTORY include DESTINATION .)
install(TARGETS imgui-ctr)

if(${IMGUI_CTR_BUILD_EXAMPLE})
    add_subdirectory(example)
endif()