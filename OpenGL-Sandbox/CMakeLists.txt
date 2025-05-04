cmake_minimum_required(VERSION 3.10)

project(OpenGL-Sandbox LANGUAGES CXX)

# Set the C++ standard and static runtime
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")

# Add the executable target
file(GLOB_RECURSE SRC_FILES "src/*.cpp" "src/*.h")

add_executable(OpenGL-Sandbox
    ${SRC_FILES}
)

# Include directories
target_include_directories(OpenGL-Sandbox PUBLIC
    ../OpenGL-Core/vendor/spdlog/include
    ../OpenGL-Core/src
    ../OpenGL-Core/vendor
    ../OpenGL-Core/${IncludeDir_glm}
    ../OpenGL-Core/${IncludeDir_Glad}
    ../OpenGL-Core/${IncludeDir_ImGui}
)

# Link libraries
target_link_libraries(OpenGL-Sandbox PRIVATE
    OpenGL-Core
)

# Platform-specific settings
if(WIN32)
    target_compile_definitions(OpenGL-Sandbox PRIVATE
        GLCORE_PLATFORM_WINDOWS
    )
endif()

# Configuration-specific settings
target_compile_definitions(OpenGL-Sandbox PRIVATE
    $<$<CONFIG:Debug>:GLCORE_DEBUG>
    $<$<CONFIG:Release>:GLCORE_RELEASE>
)

target_compile_options(OpenGL-Sandbox PRIVATE
    $<$<CONFIG:Debug>:-g>
    $<$<CONFIG:Release>:-O3>
)

# Output directories
set_target_properties(OpenGL-Sandbox PROPERTIES
    ARCHIVE_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin/${CMAKE_BUILD_TYPE}/OpenGL-Sandbox"
    LIBRARY_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin/${CMAKE_BUILD_TYPE}/OpenGL-Sandbox"
    RUNTIME_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/bin/${CMAKE_BUILD_TYPE}/OpenGL-Sandbox"
)