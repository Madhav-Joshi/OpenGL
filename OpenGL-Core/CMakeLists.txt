cmake_minimum_required(VERSION 3.10)

project(OpenGL-Core LANGUAGES CXX)

# Set the C++ standard and static runtime
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")

# Add the static library target
file(GLOB_RECURSE SRC_FILES "src/*.cpp" "src/*.h")
file(GLOB_RECURSE STB_FILES "vendor/stb_image/*.cpp" "vendor/stb_image/*.h")
file(GLOB_RECURSE GLM_FILES "vendor/glm/glm/*.hpp" "vendor/glm/glm/*.inl")

add_library(OpenGL-Core STATIC
    src/glpch.cpp
    src/glpch.h
    ${SRC_FILES}
    ${STB_FILES}
    ${GLM_FILES}
)

# Precompiled header
target_precompile_headers(OpenGL-Core PRIVATE src/glpch.h)

# Include directories
target_include_directories(OpenGL-Core PUBLIC
    src
    vendor/spdlog/include
    ${IncludeDir_GLFW}
    ${IncludeDir_Glad}
    ${IncludeDir_ImGui}
    ${IncludeDir_glm}
    ${IncludeDir_stb_image}
)

# Define macros
target_compile_definitions(OpenGL-Core PRIVATE
    _CRT_SECURE_NO_WARNINGS
)

# Link libraries
target_link_libraries(OpenGL-Core PRIVATE
    glfw
    Glad
    ImGui
    opengl32
)

# Platform-specific settings
if(WIN32)
    target_compile_definitions(OpenGL-Core PRIVATE
        GLCORE_PLATFORM_WINDOWS
        GLFW_INCLUDE_NONE
    )
endif()

# Configuration-specific settings
target_compile_definitions(OpenGL-Core PRIVATE
    $<$<CONFIG:Debug>:GLCORE_DEBUG>
    $<$<CONFIG:Release>:GLCORE_RELEASE>
)

target_compile_options(OpenGL-Core PRIVATE
    $<$<CONFIG:Debug>:-g>
    $<$<CONFIG:Release>:-O3>
)

# Output directories
set_target_properties(OpenGL-Core PROPERTIES
    ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin/${CMAKE_BUILD_TYPE}/OpenGL-Core"
    LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin/${CMAKE_BUILD_TYPE}/OpenGL-Core"
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin/${CMAKE_BUILD_TYPE}/OpenGL-Core"
)