cmake_minimum_required(VERSION 3.12 FATAL_ERROR)
project(cpp-imgui-template VERSION 0.1.0 LANGUAGES CXX)

# Install imgui
include(FetchContent)
FetchContent_Declare(
    imgui
    URL https://github.com/ocornut/imgui/archive/refs/tags/v1.88.zip
)
FetchContent_MakeAvailable(imgui)
message(STATUS "Installed imgui to ${imgui_SOURCE_DIR}")

# Install implot
include(FetchContent)
FetchContent_Declare(
    implot
    URL https://github.com/epezent/implot/archive/refs/tags/v0.13.zip
)
FetchContent_MakeAvailable(implot)
message(STATUS "Installed implot to ${implot_SOURCE_DIR}")

add_executable(${PROJECT_NAME}
    "${imgui_SOURCE_DIR}/imgui_demo.cpp"
    "${imgui_SOURCE_DIR}/imgui_draw.cpp"
    "${imgui_SOURCE_DIR}/imgui_tables.cpp"
    "${imgui_SOURCE_DIR}/imgui_widgets.cpp"
    "${imgui_SOURCE_DIR}/imgui.cpp"
    "${implot_SOURCE_DIR}/implot.cpp"
    "${implot_SOURCE_DIR}/implot_items.cpp"
    "${implot_SOURCE_DIR}/implot_demo.cpp"
)

target_include_directories(${PROJECT_NAME}
    PRIVATE "${imgui_SOURCE_DIR}"
            "${imgui_SOURCE_DIR}/backends"
            "${implot_SOURCE_DIR}"
            "${CMAKE_CURRENT_SOURCE_DIR}"
)

if (WIN32)
    target_sources(${PROJECT_NAME}
        PRIVATE win_main.cpp
                "${imgui_SOURCE_DIR}/backends/imgui_impl_dx12.cpp"
                "${imgui_SOURCE_DIR}/backends/imgui_impl_win32.cpp"
    )
    target_link_libraries(${PROJECT_NAME}
        d3d12.lib
        d3dcompiler.lib
        dxgi.lib
    )
    target_compile_definitions(${PROJECT_NAME}
        PRIVATE /nologo 
                /Zi 
                /MD 
                ImTextureID=ImU64 
                UNICODE 
                _UNICODE
    )
else()
    # TODO
endif()
