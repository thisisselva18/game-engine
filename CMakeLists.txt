cmake_minimum_required(VERSION 3.28...4.0)
project(GameEngine
    VERSION 0.0.1
    DESCRIPTION "2D Game Engine built during AIEP 2025"
    LANGUAGES CXX
)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

file(GLOB_RECURSE source src/*.cpp)
add_executable(GameEngine)
file(GLOB BMP_FILES "${CMAKE_SOURCE_DIR}/src/*.bmp")
foreach(bmp_file ${BMP_FILES})
    add_custom_command(TARGET GameEngine POST_BUILD
        COMMAND ${CMAKE_COMMAND} -E copy_if_different
        "${bmp_file}"
        "$<TARGET_FILE_DIR:GameEngine>/src"
    )
endforeach()
target_sources(GameEngine
    PRIVATE
    ${source}
)
target_compile_features(GameEngine PUBLIC cxx_std_17)
target_compile_definitions(GameEngine
                           PUBLIC SDL_MAIN_USE_CALLBACKS
                           PUBLIC IMGUI_ENABLE_DOCKING=
                           PUBLIC IMGUI_DEFINE_MATH_OPERATORS=
                           PUBLIC IMGUI_IMPL_API=
)

target_compile_options(GameEngine
    PUBLIC
    $<$<CXX_COMPILER_ID:MSVC>:/W4>
    $<$<NOT:$<CXX_COMPILER_ID:MSVC>>:-Wall -Wextra -Wpedantic -Wno-unused-parameter>
)
target_include_directories(GameEngine PRIVATE include)
target_link_libraries(GameEngine
    PRIVATE
    SDL3::SDL3
    SDL3_ttf::SDL3_ttf
    imgui_sdl3
)

set_target_properties(GameEngine
    PROPERTIES
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin"
)

file(GLOB BMP_FILES "${CMAKE_SOURCE_DIR}/src/*.bmp")

add_custom_command(TARGET GameEngine POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E copy_if_different
    "${CMAKE_SOURCE_DIR}/src"
    "${CMAKE_BINARY_DIR}/bin/src"
    "${SDL3_DLL_PATH}"
    "$<TARGET_FILE_DIR:GameEngine>"
)
